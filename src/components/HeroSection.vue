<template>
  <section class="hero">
    <div class="hero-overlay"></div>

    <div class="hero-content">
      <h1 class="main-title">WEDDING DAY</h1>
      <div class="details">
        <p class="names">Николай & Оксана</p>
        <p class="date">23.10.2026</p>
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
  background-image: url('@/assets/hero-main.jpg');
  background-size: cover;
  background-position: center 20%;
  background-repeat: no-repeat;
  color: white;
  text-align: center;
  overflow: hidden;
}

/* Фото вертикальное — на широких десктопных экранах background-size: cover
   обрезает его почти целиком. Переключаемся на contain, чтобы фото было
   видно полностью, а свободные поля по бокам заливаем тёмным фоном. */
@media (min-width: 1024px) {
  .hero {
    background-size: contain;
    background-position: center;
    background-color: var(--vt-c-black);
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
  gap: 3.5rem;
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
  margin-top: 2rem;
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

  .hero-content {
    gap: 2.5rem;
  }
}

@media (min-width: 1024px) {
  .hero-content {
    position: absolute;
    inset: 0;
    margin-top: 0;
    gap: 0;
  }

  .main-title {
    position: absolute;
    top: 1%;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
  }

  .details {
    position: absolute;
    bottom: 3%;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
    margin-top: 0;
  }

  .names {
    white-space: nowrap;
  }
}

@media (max-width: 600px) {
  .hero-content {
    position: absolute;
    inset: 0;
    margin-top: 0;
    gap: 0;
  }

  .main-title {
    position: absolute;
    top: 11%;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
  }

  .details {
    position: absolute;
    top: 82%;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
    margin-top: 0;
  }

  .names {
    font-size: 2.2rem;
    white-space: nowrap;
  }

  .date {
    font-size: 1.4rem;
  }
}
</style>