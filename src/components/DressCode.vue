<template>
  <section class="dress-code">
    <div class="container">
      <h2 class="section-title">Дресс-код</h2>
      <p class="description">Мы будем рады видеть Вас на нашей свадьбе. У нашего мероприятия дресс-кода нет, однако просим Вас (особенно женский пол) не включать в свои образы следующие цвета.</p>
      <p class="description forbidden-caption">Единственная просьба — пожалуйста, избегайте в образе этих цветов:</p>
      <div class="palette">
        <div v-for="(colorItem, index) in forbiddenColors"
             :key="index"
             class="swatch-wrapper"
             :style="{ transitionDelay: `${index * 50}ms` }">
          <div class="swatch-frame">
            <img :src="colorItem.src" class="swatch" :alt="`Запрещённый цвет: ${colorItem.name}`" />
            <svg class="forbidden-icon" viewBox="0 0 24 24">
              <circle cx="12" cy="12" r="10" fill="none" stroke="white" stroke-width="4" />
              <line x1="5.5" y1="18.5" x2="18.5" y2="5.5" stroke="white" stroke-width="4" stroke-linecap="round" />
              <circle cx="12" cy="12" r="10" fill="none" stroke="#b3122e" stroke-width="2" />
              <line x1="5.5" y1="18.5" x2="18.5" y2="5.5" stroke="#b3122e" stroke-width="2" stroke-linecap="round" />
            </svg>
          </div>
          <span class="swatch-name">{{ colorItem.name }}</span>
        </div>
      </div>
    </div>

    <div class="video-block">
      <div class="video-caption">
        <p class="video-caption-text">Почему не стоит пренебрегать дресс-кодом</p>
        <svg class="video-caption-arrow video-caption-arrow--horizontal" viewBox="0 0 120 60" fill="none">
          <path d="M4 8 C 40 4, 70 30, 108 30" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" fill="none" />
          <path d="M92 19 L 108 30 L 90 35" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" fill="none" />
        </svg>
        <svg class="video-caption-arrow video-caption-arrow--vertical" viewBox="0 0 60 90" fill="none">
          <path d="M10 4 C 4 30, 30 55, 30 82" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" fill="none" />
          <path d="M19 68 L 30 82 L 39 66" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" fill="none" />
        </svg>
      </div>

      <div class="video-player" :class="{ 'is-playing': isVideoPlaying }">
        <video
          ref="dressCodeVideo"
          class="video-el"
          src="@/assets/dress-code/video/IMG_9196.mp4"
          playsinline
          :controls="isVideoPlaying"
          @play="isVideoPlaying = true"
          @pause="isVideoPlaying = false"
          @ended="isVideoPlaying = false"
        ></video>
        <button
          v-if="!isVideoPlaying"
          class="video-play-btn"
          type="button"
          aria-label="Воспроизвести видео"
          @click="playVideo"
        >
          <svg viewBox="0 0 24 24" class="video-play-icon">
            <polygon points="6,4 20,12 6,20" fill="currentColor" />
          </svg>
        </button>
      </div>
    </div>

    <ImageGallery
      :images="currentGalleryImages"
      :is-open="isGalleryOpen"
      @close="closeGallery"
    />
  </section>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import ImageGallery from './ImageGallery.vue'
import VolumeIcon from '@/components/icons/VolumeIcon.vue'
import backgroundMusicMen from '@/assets/1050761114_1_tiktok_69f75785db7821_01365855.mp3'
import backgroundMusicWomen from '@/assets/-4884169657562792318 (audio-extractor.net).mp3'

// Запрещённые для гостей цвета в образе
import colorWhite from '@/assets/dress-code/color/5258207226910942455.jpg'
import colorBlack from '@/assets/dress-code/color/5258326910469612733.jpg'
import colorRed from '@/assets/dress-code/color/5258326910469612735.jpg'

const forbiddenColors = [
  { src: colorWhite, name: 'Белый' },
  { src: colorBlack, name: 'Чёрный' },
  { src: colorRed, name: 'Красный' },
]

// Gallery images - using glob import if possible or manual
// Since I can't easily glob with Vite in this environment without seeing the setup, I'll list them or use a helper
const womenImages = Object.values(import.meta.glob('@/assets/dress-code/ledies/*.JPG', { eager: true, import: 'default' }))
const menImages = Object.values(import.meta.glob('@/assets/dress-code/men/*.JPG', { eager: true, import: 'default' }))

const isGalleryOpen = ref(false)
const galleryType = ref('women')
const isMuted = ref(false)
const audioMen = ref(null)
const audioWomen = ref(null)

const dressCodeVideo = ref(null)
const isVideoPlaying = ref(false)

const playVideo = () => {
  if (dressCodeVideo.value) {
    dressCodeVideo.value.play().catch(e => console.log('Video play failed:', e))
  }
}


onMounted(() => {
  audioMen.value = new Audio(backgroundMusicMen)
  audioMen.value.loop = true
  audioWomen.value = new Audio(backgroundMusicWomen)
  audioWomen.value.loop = true
})

const currentGalleryImages = computed(() => {
  return galleryType.value === 'women' ? womenImages : menImages
})

const openGallery = (type) => {
  galleryType.value = type
  isGalleryOpen.value = true
  
  if (isMuted.value) return

  const currentAudio = type === 'women' ? audioWomen.value : audioMen.value
  if (currentAudio) {
    currentAudio.currentTime = 0
    currentAudio.play().catch(e => console.log('Audio play failed:', e))
  }
}

const toggleMute = () => {
  isMuted.value = !isMuted.value
  if (isMuted.value) {
    if (audioMen.value) audioMen.value.pause()
    if (audioWomen.value) audioWomen.value.pause()
  } else if (isGalleryOpen.value) {
    const currentAudio = galleryType.value === 'women' ? audioWomen.value : audioMen.value
    if (currentAudio) {
      currentAudio.play().catch(e => console.log('Audio play failed:', e))
    }
  }
}

const closeGallery = () => {
  isGalleryOpen.value = false
  if (audioMen.value) {
    audioMen.value.pause()
    audioMen.value.currentTime = 0
  }
  if (audioWomen.value) {
    audioWomen.value.pause()
    audioWomen.value.currentTime = 0
  }
}
</script>

<style scoped>
.dress-code {
  padding: 4rem 1rem;
  background-color: var(--color-background);
  text-align: center;
  color: var(--color-text);
}

.container {
  max-width: 800px;
  margin: 0 auto;
}

.section-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 2.5rem;
  margin-bottom: 1rem;
}

.description {
  font-size: 1.1rem;
  margin-bottom: 2rem;
  line-height: 1.6;
  opacity: 0.9;
}

.forbidden-caption {
  font-weight: 600;
  color: var(--color-primary);
  margin-bottom: 1.5rem;
}

.palette {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
  justify-items: center;
  margin-bottom: 2.5rem;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
}

.swatch-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.6rem;
  transition: transform 0.3s ease;
}

.swatch-wrapper:hover {
  transform: translateY(-5px);
}

.swatch-frame {
  position: relative;
  width: 90px;
  height: 90px;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  border: 3px solid var(--color-background);
  outline: 2px solid var(--color-primary);
}

.swatch {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.forbidden-icon {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  filter: drop-shadow(0 1px 2px rgba(0, 0, 0, 0.4));
}

.swatch-name {
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--color-heading);
}

.video-block {
  max-width: 700px;
  margin: 2rem auto 0;
  padding: 0 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1.5rem;
}

.video-caption {
  flex: 0 0 170px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.video-caption-text {
  font-family: 'Cormorant Garamond', serif;
  font-style: italic;
  font-size: 1.2rem;
  line-height: 1.35;
  color: var(--color-primary);
  text-align: center;
  margin: 0;
}

.video-caption-arrow {
  color: var(--color-primary);
  opacity: 0.8;
  flex-shrink: 0;
}

.video-caption-arrow--horizontal {
  width: 100px;
  height: 50px;
}

.video-caption-arrow--vertical {
  display: none;
  width: 40px;
  height: 60px;
}

.video-player {
  position: relative;
  width: 100%;
  aspect-ratio: 9 / 16;
  max-height: 640px;
  margin: 0 auto;
  border-radius: 18px;
  overflow: hidden;
  background: #000;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.25);
  outline: 2px solid var(--color-primary);
  outline-offset: 4px;
}

.video-el {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  background: #000;
}

.video-play-btn {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.25);
  border: none;
  padding: 0;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.video-play-btn:hover {
  background: rgba(0, 0, 0, 0.35);
}

.video-play-btn::before {
  content: '';
  position: absolute;
  width: 84px;
  height: 84px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(4px);
  border: 1px solid rgba(255, 255, 255, 0.6);
  transition: transform 0.3s ease;
}

.video-play-btn:hover::before {
  transform: scale(1.08);
}

.video-play-icon {
  position: relative;
  width: 34px;
  height: 34px;
  color: #fff;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.4));
}

.actions {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  flex-wrap: wrap;
}

.mute-control {
  display: flex;
  align-items: center;
}

.mute-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.3s ease;
  border: 1px solid transparent;
}

.mute-btn:hover {
  background-color: rgba(0, 0, 0, 0.05);
  border-color: var(--color-text);
}

.btn-outline {
  padding: 0.8rem 1.5rem;
  background: transparent;
  border: 1px solid var(--color-text);
  color: var(--color-text);
  font-family: inherit;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  border-radius: 4px;
}

.btn-outline:hover {
  background: var(--color-text);
  color: var(--color-background);
}

@media (max-width: 768px) {
  .palette {
    gap: 1.2rem;
  }

  .swatch-frame {
    width: 75px;
    height: 75px;
  }

  .video-block {
    flex-direction: column;
    gap: 0.25rem;
  }

  .video-caption {
    flex: 0 0 auto;
    gap: 0.25rem;
  }

  .video-caption-arrow--horizontal {
    display: none;
  }

  .video-caption-arrow--vertical {
    display: block;
  }

  .actions {
    flex-direction: column;
    align-items: center;
    gap: 1rem;
  }

  .mute-control {
    order: -1; /* Кнопка звука сверху на мобилках */
    margin-bottom: 0.5rem;
  }
  
  .btn-outline {
    width: 100%;
    max-width: 300px;
  }
}

@media (max-width: 480px) {
  .palette {
    gap: 0.8rem;
  }
  
  .swatch-frame {
    width: 64px;
    height: 64px;
  }
}
</style>