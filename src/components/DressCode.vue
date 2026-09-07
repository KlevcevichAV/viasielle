<template>
  <section class="dress-code">
    <div class="container">
      <h2 class="section-title">Дресс-код</h2>
      <p class="description">Для нас важно ваше присутствие, и нам будет очень приятно, если ваши образы поддержат атмосферу нашего дня</p>
      <div class="palette">
        <div v-for="(image, index) in images" 
             :key="index" 
             class="swatch-wrapper"
             :style="{ transitionDelay: `${index * 50}ms` }">
          <img :src="image" class="swatch" alt="Wedding color" />
        </div>
      </div>

      <div class="actions">
        <div class="mute-control">
          <button class="mute-btn" @click="toggleMute" :title="isMuted ? 'Включить звук' : 'Выключить звук'">
            <VolumeIcon :muted="isMuted" :size="24" color="var(--color-text)" />
          </button>
        </div>
        <button class="btn-outline" @click="openGallery('women')">
          Примеры образов для девушек
        </button>
        <button class="btn-outline" @click="openGallery('men')">
          Примеры образов для мужчин
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

// Color palette images
import img1 from '@/assets/dress-code/color/5332410083638811325.webp'
import img2 from '@/assets/dress-code/color/5332410083638811329.webp'
import img3 from '@/assets/dress-code/color/5332410083638811335.webp'
import img4 from '@/assets/dress-code/color/5332410083638811340.webp'
import img5 from '@/assets/dress-code/color/5332410083638811343.webp'
import img6 from '@/assets/dress-code/color/5332410083638811346.webp'
import img7 from '@/assets/dress-code/color/5332410083638811347.webp'
import img8 from '@/assets/dress-code/color/5332410083638811349.webp'
import img9 from '@/assets/dress-code/color/5332410083638811351.webp'

const images = [img1, img2, img3, img4, img5, img6, img7, img8, img9]

// Gallery images - using glob import if possible or manual
// Since I can't easily glob with Vite in this environment without seeing the setup, I'll list them or use a helper
const womenImages = Object.values(import.meta.glob('@/assets/dress-code/ledies/*.JPG', { eager: true, import: 'default' }))
const menImages = Object.values(import.meta.glob('@/assets/dress-code/men/*.JPG', { eager: true, import: 'default' }))

const isGalleryOpen = ref(false)
const galleryType = ref('women')
const isMuted = ref(false)
const audioMen = ref(null)
const audioWomen = ref(null)

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
  transition: transform 0.3s ease;
}

.swatch-wrapper:hover {
  transform: translateY(-5px);
}

.swatch {
  width: 70px;
  height: 85px;
  object-fit: contain;
  filter: drop-shadow(0 4px 6px rgba(0, 0, 0, 0.1));
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
  
  .swatch {
    width: 60px;
    height: 75px;
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
  
  .swatch {
    width: 50px;
    height: 65px;
  }
}
</style>