<template>
  <section class="wishes">
    <div class="container">
      <h2 class="section-title">Детали</h2>
      <p class="description">
        Пожалуйста, не ломайте голову над выбором подарка! Наши вкусы настолько специфичны, что угодить нам могут только
        деньги!
      </p>

      <div class="wine-text-container">
        <WineBottleIcon :size="35" :rotate="-15" color="#78866b" class="wine-icon" />
        <p class="description wine-description">
          Чтобы наш праздник оставил долгое послевкусие, мы решили собрать «Свадебную винотеку».<br>Вместо цветов, которые быстро отцветают, будем рады получить от вас бутылку вина. Каждая из них станет поводом вспомнить о вас и о нашем особенном дне в будущем.
        </p>
      </div>
    </div>
  </section>
</template>

<script setup>
import WineBottleIcon from '@/components/icons/WineBottleIcon.vue'
// Используем Vue 3
</script>

<style scoped>
.wishes {
  padding: 4rem 1rem;
  background-color: var(--color-background);
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
  text-align: center;
}

.description {
  text-align: center; /* Текст описания оставляем по центру */
  font-size: 1.1rem;
  margin-top: 0;
  line-height: 1.6;
  opacity: 0.9;
}

.wine-text-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  margin-top: 3rem;
  position: relative;
}

.wine-description {
  margin-top: 0; /* Убираем верхний отступ, так как он есть у контейнера */
  max-width: 600px;
}

.wine-icon {
  flex-shrink: 0;
}

@media (max-width: 768px) {
  .section-title {
    font-size: 2.5rem;
  }
}

@media (max-width: 600px) {
  .wine-text-container {
    flex-direction: column;
    gap: 0.5rem;
  }
}
</style>