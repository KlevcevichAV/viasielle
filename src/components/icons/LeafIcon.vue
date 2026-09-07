<template>
  <div 
    class="leaf-icon"
    :style="{ 
      width: size + 'px', 
      height: 'auto',
      transform: `rotate(${rotate}deg)`,
      display: 'inline-block'
    }"
  >
    <img 
      :src="imageSrc" 
      alt="leaf" 
      :style="{ width: '100%', height: 'auto', display: 'block' }"
    />
  </div>
</template>

<script setup>
import { computed } from 'vue'
import leaf1 from '@/assets/Screenshot_2026-05-04_at_3.04.16_PM-removebg-preview.png'
import leaf2 from '@/assets/Screenshot_2026-05-04_at_3.04.21_PM-removebg-preview.png'
import leaf3 from '@/assets/Screenshot_2026-05-04_at_3.04.24_PM-removebg-preview.png'

const props = defineProps({
  size: { type: [Number, String], default: 40 },
  color: { type: String, default: 'currentColor' }, // Оставляем для совместимости, но для PNG не используем
  rotate: { type: [Number, String], default: 0 },
  variant: { type: Number, default: 1 }
})

const imageSrc = computed(() => {
  switch (props.variant) {
    case 1: return leaf1
    case 2: return leaf2
    case 3: return leaf3
    default: return leaf1
  }
})
</script>

<style scoped>
.leaf-icon {
  line-height: 0;
}
/* Инверсия цвета для темной темы, если нужно, хотя PNG уже имеют свой цвет */
[data-theme='dark'] img {
  filter: brightness(1.2) sepia(0.2);
}
</style>
