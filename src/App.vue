<template>
  <main class="wedding-app">
    <div class="theme-toggle">
      <button @click="toggleTheme" class="toggle-button" :title="isDark ? 'Switch to Light' : 'Switch to Dark'">
        <span v-if="isDark">☀️</span>
        <span v-else>🌙</span>
      </button>
    </div>

    <button
        @click="isMenuOpen = !isMenuOpen"
        :class="['burger-button', { 'is-active': isMenuOpen }]"
        aria-label="Menu"
    >
      <span class="line"></span>
      <span class="line"></span>
      <span class="line"></span>
    </button>

    <nav :class="['side-menu', { 'is-open': isMenuOpen }]">
      <div class="menu-links">
        <a href="#hero" @click="isMenuOpen = false">Главная</a>
        <a href="#program" @click="isMenuOpen = false">Программа</a>
        <a href="#location" @click="isMenuOpen = false">Место проведения</a>
        <a href="#dresscode" @click="isMenuOpen = false">Дресс код</a>
        <a href="#rsvp" @click="isMenuOpen = false">Анкета</a>
        <a href="#wishes" @click="isMenuOpen = false">Детали</a>
        <a href="#chat" @click="isMenuOpen = false">Общий чат</a>
        <a href="#final" @click="isMenuOpen = false">Финал</a>
      </div>
    </nav>

    <div :class="['content-wrapper', { 'blurred': isMenuOpen }]">
      <DecorativeDecor />
      <HeroSection id="hero"/>
      <CountdownTimer id="timer"/>
      <ProgramSection id="program"/>
      <LocationSection id="location"/>
      <DressCode id="dresscode"/>
      <RSVPForm id="rsvp"/>
      <WishesSection id="wishes"/>
      <JoinToChat id="chat"/>
      <FinalSection id="final"/>
    </div>
  </main>
</template>

<script setup>
import {ref, onMounted, watch} from 'vue'
import HeroSection from './components/HeroSection.vue'
import ProgramSection from './components/ProgramSection.vue'
import LocationSection from './components/LocationSection.vue'
import DressCode from './components/DressCode.vue'
import RSVPForm from './components/RSVPForm.vue'
import CountdownTimer from './components/CountdownTimer.vue'
import JoinToChat from "@/components/JoinToChat.vue";
import WishesSection from "@/components/WishesSection.vue";
import FinalSection from "@/components/FinalSection.vue";
import DecorativeDecor from "@/components/DecorativeDecor.vue";

const isMenuOpen = ref(false)

watch(isMenuOpen, (val) => {
  if (val) {
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = ''
  }
})
const isDark = ref(false)

const toggleTheme = () => {
  isDark.value = !isDark.value
  const theme = isDark.value ? 'dark' : 'light'
  document.documentElement.setAttribute('data-theme', theme)
  localStorage.setItem('theme', theme)
}

onMounted(() => {
  isDark.value = document.documentElement.getAttribute('data-theme') === 'dark'
})
</script>

<style scoped>
/* Кнопка смены темы */
.theme-toggle {
  position: fixed;
  top: 2rem;
  left: 2rem;
  z-index: 200;
}

.toggle-button {
  background: var(--color-background-soft);
  border: 1px solid var(--color-border);
  border-radius: 50%;
  width: 45px;
  height: 45px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 1.5rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.toggle-button:hover {
  transform: scale(1.1);
  border-color: var(--color-primary);
}

/* Кнопка бургера */
.burger-button {
  position: fixed;
  top: 2rem;
  right: 2rem;
  z-index: 200;
  width: 40px;
  height: 30px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
}

.line {
  width: 100%;
  height: 3px;
  background-color: var(--color-heading);
  transition: all 0.3s ease-in-out;
}

.burger-button.is-active .line:nth-child(1) {
  transform: translateY(13.5px) rotate(45deg);
}

.burger-button.is-active .line:nth-child(2) {
  opacity: 0;
}

.burger-button.is-active .line:nth-child(3) {
  transform: translateY(-13.5px) rotate(-45deg);
}

.side-menu {
  position: fixed;
  top: 0;
  right: 0;
  width: 100%;
  height: 100vh;
  background-color: var(--color-menu-bg);
  z-index: 150;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  transition: transform 0.5s cubic-bezier(0.77, 0, 0.175, 1);
  transform: translateX(100%);
  overflow-y: auto;
}

.side-menu.is-open {
  transform: translateX(0);
}

.menu-links {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  text-align: center;
  padding: 2rem 2rem 3rem;
  width: 100%;
}

.menu-links a {
  font-size: 2.5rem;
  text-decoration: none;
  color: var(--color-secondary);
  font-family: 'Cormorant Garamond', serif;
  transition: opacity 0.3s;
  padding: 0.5rem;
}

@media (min-width: 1024px) {
  .menu-links a {
    font-size: 3rem;
  }
  .side-menu {
    align-items: flex-start;
  }
  .menu-links {
    padding: 1rem 2rem 3rem;
  }
}

.menu-links a:hover {
  opacity: 0.6;
}

.content-wrapper {
  transition: filter 0.5s ease;
  position: relative;
  min-height: 100vh;
}

.content-wrapper.blurred {
  filter: blur(5px);
  pointer-events: none;
}
</style>