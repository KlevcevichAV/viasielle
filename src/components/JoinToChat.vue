<template>
  <section class="dress-code">
    <div class="container">
      <h2 class="section-title">Общий чат гостей</h2>

      <p class="description">
        Место для ваших улыбок, живых кадров и непринужденных видео. Будем счастливы увидеть наш праздник вашими глазами. Заходите в чат и делитесь моментами!
        <HeartIcon :size="18" color="#78866b" filled class="heart-inline" />
      </p>

      <a
          href="https://t.me/+FsZnUO1HrRwzYTI6"
          target="_blank"
          class="telegram-button"
      >
        <!-- Telegram SVG -->
        <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            class="telegram-icon"
        >
          <path
              fill="currentColor"
              d="M9.993 15.674l-.396 3.945c.567 0 .813-.244 1.11-.537l2.664-2.528 5.52 4.035c1.013.558 1.732.264 2.004-.939l3.632-17.04h.001c.32-1.495-.54-2.079-1.525-1.71L1.57 9.36C.146 9.918.168 10.7 1.324 11.06l5.83 1.82 13.54-8.54c.637-.41 1.217-.183.74.227"
          />
        </svg>

        Присоединиться к чату
      </a>
    </div>
  </section>
</template>

<script setup>
import HeartIcon from './icons/HeartIcon.vue'
</script>

<style scoped>
.dress-code {
  padding: 4rem 1rem;
  background-color: var(--color-background-soft);
  text-align: center;
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
}

.description {
  font-size: 1.1rem;
  margin-bottom: 2rem;
  line-height: 1.6;
  opacity: 0.9;
}

.heart-inline {
  display: inline-block;
  vertical-align: middle;
  margin-left: 4px;
  position: relative;
  top: -1px;
}

.telegram-button {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;

  padding: 0.9rem 1.6rem;
  border-radius: 999px;

  background: #78866b;
  color: white;
  font-weight: 500;
  text-decoration: none;

  transition: all 0.25s ease;
  box-shadow: 0 4px 12px rgba(120, 134, 107, 0.3);
}

.telegram-button:hover {
  transform: translateY(-2px);
  background: #6a7860; /* чуть темнее при ховере */
  box-shadow: 0 6px 18px rgba(120, 134, 107, 0.4);
}

.telegram-icon {
  width: 20px;
  height: 20px;
}

</style>