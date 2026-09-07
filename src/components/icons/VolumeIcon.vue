<template>
  <svg :width="size" :height="size" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" :style="{ transform: `rotate(${rotate}deg)` }">
    <path v-if="muted" d="M11 5L6 9H2V15H6L11 19V5Z" :stroke="color" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
    <path v-if="muted" d="M23 9L17 15" :stroke="color" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
    <path v-if="muted" d="M17 9L23 15" :stroke="color" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
    
    <path v-if="!muted" d="M11 5L6 9H2V15H6L11 19V5Z" :stroke="color" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
    <path v-if="!muted" d="M19.07 4.93C20.9447 6.80528 21.9979 9.34836 21.9979 12C21.9979 14.6516 20.9447 17.1947 19.07 19.07" :stroke="color" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
    <path v-if="!muted" d="M15.54 8.46C16.4771 9.39764 17.004 10.6692 17.004 12C17.004 13.3308 16.4771 14.6024 15.54 15.54" :stroke="color" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
  </svg>
</template>

<script setup>
defineProps({
  size: { type: [Number, String], default: 24 },
  color: { type: String, default: 'currentColor' },
  rotate: { type: [Number, String], default: 0 },
  muted: { type: Boolean, default: false }
})
</script>
