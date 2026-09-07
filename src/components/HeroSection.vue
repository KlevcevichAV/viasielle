<template>
  <section class="hero">
    <div class="hero-overlay"></div>

    <div class="hero-content">
      <h1 class="main-title">WEDDING DAY</h1>
      <div class="details">
        <p class="names">Александр & Маргарита</p>
        <p class="date">04.09.2026</p>
      </div>
    </div>
  </section>
</template>

<script setup>
// Компонент Vue 3 с использованием script setup
</script>

<style scoped>
/* Подключаем элегантный шрифт с засечками */
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@300;400;500;600&display=swap');

.hero {
  position: relative;
  height: 100vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  /* Используем твое фоновое изображение */
  background-image: url('@/assets/IMG_9362.JPG');
  background-size: cover;
  background-position: center 20%;
  color: white;
  text-align: center;
  overflow: hidden;
}

@media (min-width: 1024px) {
  .hero {
    background-position: center 25%;
  }
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: var(--color-overlay);
}

.hero-content {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  gap: 1.2rem;
  margin-top: -4rem;
}

.main-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 5rem;
  font-weight: 500;
  letter-spacing: 0.15em;
  margin: 0;
  line-height: 1.1;
  text-transform: uppercase;
}

.details {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.names {
  font-family: 'Cormorant Garamond', serif;
  font-size: 4.8rem;
  font-weight: 400;
  margin: 0;
}

.date {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.8rem;
  font-weight: 500;
  letter-spacing: 0.1em;
  margin: 0;
}

/* Адаптивность под мобильные устройства */
@media (max-width: 1024px) {
  .main-title {
    font-size: 3.5rem;
  }

  .names {
    font-size: 3rem;
  }
}

@media (max-width: 600px) {
  .names {
    font-size: 2.2rem;
    white-space: nowrap;
  }

  .date {
    font-size: 1.4rem;
  }
}
</style>