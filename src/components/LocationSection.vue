<template>
  <section class="map-section">
    <h2 class="map-title">Место проведения</h2>

    <!-- Новый блок с локацией -->
    <div class="location-info">
      <p class="location-name">TARASOVO VILLAGE</p>
      <p class="location-address">(д. Тарасово, ул. Олимпийская, 3)</p>
    </div>

    <div class="location-gallery">
      <div v-for="(img, index) in locationImages" :key="index" class="gallery-item">
        <img :src="img" alt="Location photo" class="location-photo" />
      </div>
    </div>

    <div class="map-card">

      <iframe
          src="https://yandex.ru/map-widget/v1/?um=constructor%3Ac1f3bfe9e9e99a1c16a4ce603e12a8291885df5c033615875bb2814b123d4928&amp;source=constructor"
          width="100%"
          height="450"
          frameborder="0"
          allowfullscreen="true"
          style="position:relative;">
      </iframe>
    </div>
  </section>
</template>

<script setup>
const locationImages = Object.values(import.meta.glob('@/assets/location/*.{webp,jpeg,JPG,png}', { eager: true, import: 'default' }))
</script>

<style scoped>
.map-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 60px 20px;
  background-color: var(--color-background-soft);
}

.map-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 2.2rem;
  margin-bottom: 25px;
  color: var(--color-heading);
}

/* Новый блок */
.location-info {
  text-align: center;
  margin-bottom: 25px;
}

.location-name {
  font-family: 'Montserrat', sans-serif;
  font-size: 1.2rem;
  font-weight: 600;
  color: var(--color-heading);
}

.location-address {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.95rem;
  color: var(--color-text);
  opacity: 0.8;
}

.location-gallery {
  display: flex;
  gap: 20px;
  justify-content: center;
  margin-bottom: 30px;
  width: 100%;
  max-width: 1400px;
  flex-wrap: wrap;
}

.gallery-item {
  flex: 1;
  min-width: 300px;
  max-width: 700px;
  height: 400px;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.location-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.location-photo:hover {
  transform: scale(1.05);
}

.map-card {
  width: 80%;
  max-width: 500px;
  background-color: var(--color-background);
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.15);
  border: 8px solid var(--color-background);
}

@media (max-width: 768px) {
  .map-section {
    padding: 40px 0;
  }

  .map-card {
    border-radius: 10px;
  }

  .map-card iframe {
    height: 350px;
  }

  .gallery-item {
    min-width: 100%;
    height: 250px;
  }

  .location-gallery {
    gap: 15px;
    padding: 0 10px;
  }
}
</style>