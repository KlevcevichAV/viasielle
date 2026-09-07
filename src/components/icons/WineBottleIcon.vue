<template>
  <div
    class="wine-bottle-icon"
    :style="{
      width: size + 'px',
      height: 'auto',
      transform: `rotate(${rotate}deg)`,
      display: 'inline-block'
    }"
  >
    <svg
      viewBox="0 0 100 300"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      :style="{ width: '100%', height: 'auto', display: 'block' }"
    >
      <defs>
        <linearGradient :id="`cork-${uid}`" x1="0" y1="0" x2="1" y2="0">
          <stop offset="0%" stop-color="#e6c79c" />
          <stop offset="55%" stop-color="#c99a63" />
          <stop offset="100%" stop-color="#8a5a34" />
        </linearGradient>
        <linearGradient :id="`amber-${uid}`" x1="0" y1="0" x2="1" y2="0">
          <stop offset="0%" stop-color="#c97a2b" />
          <stop offset="35%" stop-color="#f2a94a" />
          <stop offset="60%" stop-color="#d6822f" />
          <stop offset="100%" stop-color="#8a4a12" />
        </linearGradient>
        <clipPath :id="`clip-${uid}`">
          <rect x="18" y="140" width="64" height="142" rx="10" />
        </clipPath>
      </defs>

      <!-- Тень -->
      <ellipse cx="56" cy="288" rx="42" ry="7" fill="#000000" fill-opacity="0.14" />

      <!-- Силуэт стекла: горлышко + плечи + корпус -->
      <path
        d="M42 48H58V108C58 108 76 118 78 140C80 158 80 158 80 158V272C80 281 73 288 64 288H36C27 288 20 281 20 272V158C20 158 20 158 22 140C24 118 42 108 42 108V48Z"
        fill="#e7edf3" fill-opacity="0.35" stroke="#9aa7b4" stroke-width="1.5" stroke-opacity="0.6" />

      <!-- Виски -->
      <rect x="18" y="196" width="64" height="86" :fill="`url(#amber-${uid})`" :clip-path="`url(#clip-${uid})`" />
      <!-- Блики преломления в жидкости -->
      <g :clip-path="`url(#clip-${uid})`" opacity="0.35">
        <rect x="34" y="196" width="5" height="86" fill="#5a2f08" />
        <rect x="47" y="196" width="6" height="86" fill="#fddca0" />
        <rect x="63" y="196" width="4" height="86" fill="#5a2f08" />
      </g>
      <!-- Линия поверхности жидкости -->
      <path d="M20 197C20 197 35 193 50 193C65 193 80 197 80 197" stroke="#fddca0" stroke-width="1.5" stroke-opacity="0.6" />

      <!-- Блик на стекле -->
      <path d="M27 150C25 180 25 230 27 275" stroke="white" stroke-width="4" stroke-linecap="round" stroke-opacity="0.4" />
      <path d="M46 55V102" stroke="white" stroke-width="2" stroke-linecap="round" stroke-opacity="0.35" />

      <!-- Пробка -->
      <rect x="40" y="20" width="20" height="30" rx="2" :fill="`url(#cork-${uid})`" />
      <rect x="37" y="8" width="26" height="16" rx="7" :fill="`url(#cork-${uid})`" />
      <line x1="42" y1="30" x2="58" y2="30" stroke="#6e4523" stroke-width="1.2" stroke-opacity="0.5" />
      <line x1="42" y1="38" x2="58" y2="38" stroke="#6e4523" stroke-width="1.2" stroke-opacity="0.5" />
    </svg>
  </div>
</template>

<script setup>
defineProps({
  size: { type: [Number, String], default: 40 },
  color: { type: String, default: '#b3122e' },
  rotate: { type: [Number, String], default: -15 }
})

const uid = Math.random().toString(36).slice(2)
</script>

<style scoped>
.wine-bottle-icon {
  line-height: 0;
  vertical-align: middle;
  filter: drop-shadow(0 4px 6px rgba(0, 0, 0, 0.15));
}
</style>
