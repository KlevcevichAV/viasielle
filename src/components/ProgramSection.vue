<template>
  <section class="program-section">
    <h2 class="section-title">Программа дня</h2>

    <div class="timeline-container" ref="container" :style="{ height: totalHeight + 'px' }">
      <svg class="timeline-svg" viewBox="0 0 400 1000" preserveAspectRatio="none">
        <path
            id="curve"
            d="M200,0 Q280,150 200,300 T200,600 T200,900 T200,1000"
            fill="none"
            stroke="var(--color-border)"
            stroke-width="2"
        />
        <path
            id="curve-progress"
            d="M200,0 Q280,150 200,300 T200,600 T200,900 T200,1000"
            fill="none"
            stroke="#78866b"
            stroke-width="3"
            stroke-linecap="round"
            style="stroke-dasharray: 0 10000;"
        />
        <g id="heart-pointer" ref="heart">
          <path
              d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
              fill="var(--color-primary)"
              transform="scale(1.2) translate(-12, -12)"
          />
        </g>
      </svg>

      <div
          v-for="(item, index) in itemsWithCoords"
          :key="index"
          :class="['program-item', index % 2 === 0 ? 'left' : 'right', { reached: item.reached }]"
          :style="{ left: item.x + 'px', top: item.y + 'px' }"
      >
        <div class="dot"></div>
        <div class="content">
          <span class="time">{{ item.time }}</span>
          <h3 class="title">{{ item.title }}</h3>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue';

const timeline = [
  { time: '15:30', title: 'Сбор гостей' },
  { time: '16:30', title: 'Церемония' },
  { time: '17:00', title: 'Начало банкета' },
  { time: '22:00', title: 'Праздничный торт' },
  { time: '23:00', title: 'Завершение свадьбы и начало счастливой жизни' }
];

const heart = ref(null);
const container = ref(null);
const itemsWithCoords = ref([]);
const totalHeight = ref(1000);

const calculateCoords = () => {
  const path = document.querySelector('#curve');
  if (!path || !container.value) return;

  const pathLength = path.getTotalLength();
  const containerWidth = container.value.offsetWidth;

  totalHeight.value = Math.max(800, containerWidth * 2.5);
  const currentHeight = totalHeight.value;

  const steps = [0.1, 0.3, 0.5, 0.7, 0.9];

  itemsWithCoords.value = timeline.map((item, index) => {
    const svgPoint = path.getPointAtLength(steps[index] * pathLength);
    const xPixel = (svgPoint.x / 400) * containerWidth;
    const yPixel = (svgPoint.y / 1000) * currentHeight;

    return { ...item, x: xPixel, y: yPixel, progressStep: steps[index], reached: false };
  });
};

const handleScroll = () => {
  const path = document.querySelector('#curve');
  if (!path || !heart.value || !container.value) return;

  const pathLength = path.getTotalLength();

  // 🔥 ключевая часть — считаем абсолютную позицию
  const scrollY = window.scrollY + window.innerHeight / 3;
  const containerTop =
      container.value.getBoundingClientRect().top + window.scrollY;
  const containerHeight = container.value.offsetHeight;

  // прогресс строго линейный от скролла
  let progress = (scrollY - containerTop) / containerHeight;

  // ограничиваем
  progress = Math.max(0.013, Math.min(0.943, progress));

  const point = path.getPointAtLength(progress * pathLength);
  const nextPoint = path.getPointAtLength(
      Math.min(pathLength, progress * pathLength + 1)
  );

  const angle =
      (Math.atan2(nextPoint.y - point.y, nextPoint.x - point.x) * 180) /
      Math.PI;

  heart.value.setAttribute(
      'transform',
      `translate(${point.x}, ${point.y}) rotate(${angle - 90})`
  );

  const progressPath = document.querySelector('#curve-progress');
  if (progressPath) {
    progressPath.style.strokeDasharray = `${progress * pathLength} ${pathLength}`;
  }

  // Обновляем состояние точек
  itemsWithCoords.value.forEach(item => {
    item.reached = progress >= item.progressStep;
  });
};

const handleResize = () => {
  calculateCoords();
  handleScroll();
};

onMounted(async () => {
  await nextTick();

  const init = () => {
    calculateCoords();
    handleScroll();
  };

  setTimeout(init, 200);

  window.addEventListener('scroll', handleScroll);
  window.addEventListener('resize', handleResize);
  window.addEventListener('hashchange', () => {
    setTimeout(handleScroll, 100);
  });
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
  window.removeEventListener('resize', handleResize);
});
</script>

<style scoped>
.program-section {
  padding: 100px 0;
  background-color: var(--color-background);
  overflow: hidden;
  scroll-margin-top: 50px;
}

.section-title {
  text-align: center;
  font-family: 'Cormorant Garamond', serif;
  font-size: 2.5rem;
  margin-bottom: 60px;
  color: var(--color-heading);
}

.timeline-container {
  position: relative;
  max-width: 800px;
  margin: 0 auto;
}

.timeline-svg {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.program-item {
  position: absolute;
  width: auto;
  min-width: 120px;
  max-width: 250px;
  display: flex;
  align-items: center;
  z-index: 2;
}

/* Общая стилизация точки */
.dot {
  position: absolute;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 14px;
  height: 14px;
  background-color: #a0a0a0; /* Серый цвет для "непройденных" точек */
  border-radius: 50%;
  flex-shrink: 0;
  border: 3px solid var(--color-background-soft);
  box-shadow: 0 0 0 2px #a0a0a0;
  z-index: 3;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
}

.program-item.reached .dot {
  background-color: #78866b;
  box-shadow: 0 0 0 2px #78866b;
}

/* Позиционирование элементов слева от кривой */
.program-item.left {
  text-align: right;
  justify-content: flex-end;
  transform: translate(-100%, -50%); /* Сдвигаем весь блок влево от координаты X */
  padding-right: 25px;
}

.program-item.left .dot {
  left: 100%; /* Точка на правом краю блока */
}

/* Позиционирование элементов справа от кривой */
.program-item.right {
  text-align: left;
  justify-content: flex-start;
  transform: translate(0, -50%); /* Блок начинается от координаты X */
  padding-left: 25px;
}

.program-item.right .dot {
  left: 0; /* Точка на левом краю блока */
}

.time {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--color-primary);
  display: block;
  margin-bottom: 4px;
}

.title {
  font-size: 1.2rem;
  margin: 0;
  line-height: 1.2;
  color: var(--color-heading);
}

/* Адаптив для мобильных устройств */
@media (max-width: 600px) {
  .section-title {
    font-size: 2rem;
  }

  .program-item {
    max-width: 150px; /* Еще больше места для кривой на узких экранах */
  }

  .title {
    font-size: 0.95rem;
  }

  .time {
    font-size: 1rem;
  }
}
</style>