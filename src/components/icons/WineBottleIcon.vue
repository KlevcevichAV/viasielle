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
      <!-- Силуэт бутылки -->
      <path 
        d="M38 75C38 65 40 40 40 20C40 10 43 8 50 8C57 8 60 10 60 20C60 40 62 65 62 75C62 95 85 110 85 140V270C85 285 78 292 65 292H35C22 292 15 285 15 270V140C15 110 38 95 38 75Z" 
        :fill="color"
      />
      <!-- Этикетка (светлее основного цвета) -->
      <rect x="25" y="150" width="50" height="70" rx="2" fill="white" fill-opacity="0.2" />
      <!-- Полоска на горлышке (фольга) -->
      <path d="M40 25C40 20 40 20 40 20C40 15 43 14 50 14C57 14 60 15 60 20C60 20 60 20 60 25H40Z" fill="white" fill-opacity="0.3" />
      <!-- Блик на плече -->
      <path d="M30 145C30 135 45 125 45 125" stroke="white" stroke-width="2" stroke-linecap="round" stroke-opacity="0.3" />
      <!-- Блик на корпусе -->
      <rect x="75" y="160" width="2" height="60" rx="1" fill="white" fill-opacity="0.2" />
    </svg>
  </div>
</template>

<script setup>
defineProps({
  size: { type: [Number, String], default: 40 },
  color: { type: String, default: '#78866b' },
  rotate: { type: [Number, String], default: -15 }
})
</script>

<style scoped>
.wine-bottle-icon {
  line-height: 0;
  vertical-align: middle;
}
</style>
