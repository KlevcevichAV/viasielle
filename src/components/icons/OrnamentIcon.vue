<template>
  <svg
    :width="size"
    :height="size"
    viewBox="0 0 100 100"
    xmlns="http://www.w3.org/2000/svg"
    :style="{ transform: `rotate(${rotate}deg)` }"
  >
    <!-- Rhombus with hook motifs ("ромб з рагамі") -->
    <g v-if="variant === 1" fill="none" :stroke="color" stroke-width="5" stroke-linecap="square">
      <polygon points="50,8 92,50 50,92 8,50" />
      <polygon points="50,30 70,50 50,70 30,50" :fill="color" stroke="none" />
      <path d="M50,8 L38,-4 M50,8 L62,-4 M50,92 L38,104 M50,92 L62,104" />
      <path d="M8,50 L-4,38 M8,50 L-4,62 M92,50 L104,38 M92,50 L104,62" />
    </g>

    <!-- Eight-point star ("васьміканцовая зорка") -->
    <g v-else-if="variant === 2" fill="none" :stroke="color" stroke-width="5">
      <polygon points="50,6 94,50 50,94 6,50" />
      <polygon points="24,24 76,24 76,76 24,76" />
    </g>

    <!-- Solar cross ("сонейка") -->
    <g v-else fill="none" :stroke="color" stroke-width="5" stroke-linecap="square">
      <path d="M50,6 V38 M50,62 V94 M6,50 H38 M62,50 H94" />
      <polygon points="50,38 58,50 50,62 42,50" :fill="color" stroke="none" />
      <circle cx="50" cy="50" r="34" stroke-width="3" />
    </g>
  </svg>
</template>

<script setup>
defineProps({
  size: { type: [Number, String], default: 40 },
  color: { type: String, default: 'currentColor' },
  rotate: { type: [Number, String], default: 0 },
  variant: { type: Number, default: 1 },
})
</script>
