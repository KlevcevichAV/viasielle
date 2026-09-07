<template>
  <svg :width="size" :height="size" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" :style="{ transform: `rotate(${rotate}deg)` }">
    <defs>
      <radialGradient id="heartGradient" cx="35%" cy="35%" r="55%" fx="35%" fy="35%">
        <stop offset="0%" stop-color="#ffffff" stop-opacity="0.3" />
        <stop offset="50%" stop-color="#ffffff" stop-opacity="0" />
        <stop offset="100%" stop-color="#000000" stop-opacity="0.1" />
      </radialGradient>
      <filter id="shadow" x="-20%" y="-20%" width="140%" height="140%">
        <feGaussianBlur in="SourceAlpha" stdDeviation="0.1" />
        <feOffset dx="0.1" dy="0.1" result="offsetblur" />
        <feComponentTransfer>
          <feFuncA type="linear" slope="0.05" />
        </feComponentTransfer>
        <feMerge>
          <feMergeNode />
          <feMergeNode in="SourceGraphic" />
        </feMerge>
      </filter>
    </defs>
    <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z" 
          :fill="filled ? color : 'none'" 
          :stroke="filled ? 'none' : color" 
          stroke-width="0.8" 
          stroke-linecap="round" 
          stroke-linejoin="round" />
    <path v-if="filled" d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z" 
          fill="url(#heartGradient)" 
          :filter="'url(#shadow)'" />
  </svg>
</template>

<script setup>
defineProps({
  size: { type: [Number, String], default: 24 },
  color: { type: String, default: 'currentColor' },
  rotate: { type: [Number, String], default: 0 },
  filled: { type: Boolean, default: false }
})

// Функция для изменения яркости цвета
const adjustColor = (col, amt) => {
  if (col === 'currentColor' || !col.startsWith('#')) return col === 'currentColor' ? '#ffffff' : col;
  const usePound = col[0] === "#";
  const num = parseInt(col.slice(usePound ? 1 : 0), 16);
  
  let r = (num >> 16) + amt;
  let g = ((num >> 8) & 0x00FF) + amt;
  let b = (num & 0x0000FF) + amt;
  
  const clamp = (val) => Math.min(255, Math.max(0, val));
  r = clamp(r);
  g = clamp(g);
  b = clamp(b);
  
  return (usePound ? "#" : "") + (b | (g << 8) | (r << 16)).toString(16).padStart(6, '0');
}
</script>
