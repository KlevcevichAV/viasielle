<template>
  <section class="timer-section">
    <h3 class="timer-title">До торжества осталось:</h3>

    <div v-if="!isExpired" class="timer-container">
      <div class="timer-item">
        <span class="value">{{ timeLeft.days }}</span>
        <span class="label">{{ declensions.days }}</span>
      </div>
      <div class="timer-item">
        <span class="value">{{ timeLeft.hours }}</span>
        <span class="label">{{ declensions.hours }}</span>
      </div>
      <div class="timer-item">
        <span class="value">{{ timeLeft.minutes }}</span>
        <span class="label">{{ declensions.minutes }}</span>
      </div>
      <div class="timer-item">
        <span class="value">{{ timeLeft.seconds }}</span>
        <span class="label">{{ declensions.seconds }}</span>
      </div>
    </div>

    <div v-else class="expired-msg">
      <h2>Этот счастливый день настал!</h2>
    </div>
  </section>
</template>

<script setup>
import {ref, onMounted, onUnmounted, computed} from 'vue'

const targetDate = new Date('2026-09-04T15:30:00').getTime()

const isExpired = ref(false)
const timeLeft = ref({
  days: 0,
  hours: 0,
  minutes: 0,
  seconds: 0
})

let timerInterval = null

const getDeclension = (number, titles) => {
  const cases = [2, 0, 1, 1, 1, 2]
  return titles[
    number % 100 > 4 && number % 100 < 20
      ? 2
      : cases[number % 10 < 5 ? number % 10 : 5]
  ]
}

const declensions = computed(() => ({
  days: getDeclension(timeLeft.value.days, ['день', 'дня', 'дней']),
  hours: getDeclension(timeLeft.value.hours, ['час', 'часа', 'часов']),
  minutes: getDeclension(timeLeft.value.minutes, ['минута', 'минуты', 'минут']),
  seconds: getDeclension(timeLeft.value.seconds, ['секунда', 'секунды', 'секунд'])
}))

const updateCountdown = () => {
  const now = new Date().getTime()
  const distance = targetDate - now

  // Если дата прошла
  if (distance < 0) {
    isExpired.value = true
    clearInterval(timerInterval)
    return
  }

  // Математика перевода миллисекунд в единицы времени
  timeLeft.value = {
    days: Math.floor(distance / (1000 * 60 * 60 * 24)),
    hours: Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)),
    minutes: Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60)),
    seconds: Math.floor((distance % (1000 * 60)) / 1000)
  }
}

// Жизненный цикл: запускаем при монтировании, чистим при уничтожении
onMounted(() => {
  updateCountdown()
  timerInterval = setInterval(updateCountdown, 1000)
})

onUnmounted(() => {
  if (timerInterval) clearInterval(timerInterval)
})
</script>

<style scoped>
.timer-section {
  text-align: center;
  padding: 4rem 1rem;
  background-color: var(--color-background-soft);
}

.timer-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.8rem;
  margin-bottom: 2rem;
  color: var(--color-heading);
}

.timer-container {
  display: flex;
  justify-content: center;
  gap: 2rem;
}

.timer-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 80px;
}

.value {
  font-size: 3rem;
  font-weight: 300;
  color: var(--color-primary);
  line-height: 1;
}

.label {
  font-size: 0.9rem;
  text-transform: uppercase;
  color: var(--color-text);
  margin-top: 0.5rem;
  letter-spacing: 1px;
}

/* Адаптив для мобилок */
@media (max-width: 600px) {
  .timer-container {
    gap: 1rem;
  }

  .value {
    font-size: 2rem;
  }

  .timer-item {
    min-width: 60px;
  }
}
</style>