<template>
  <section class="final-section">
    <div class="container">
      <div class="content">
        <h3 class="title">Топ-1 мест, куда сходить этой осенью: <span class="main-text">наша свадьба!</span></h3>
        <p class="sub-text">Будем очень рады разделить этот день с вами!</p>
        <div class="final-photo-container">
          <img src="@/assets/IMG_9325.JPG" alt="Александр и Маргарита" class="final-photo" />
        </div>
        <p class="signature">С любовью,<br>Александр и Маргарита</p>
      </div>
    </div>
  </section>
</template>

<script setup>
</script>

<style scoped>
.final-section {
  padding: 6rem 1rem 0.5rem 1rem;
  background-color: var(--color-background);
  text-align: center;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  padding-bottom: 0;
}

.content {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  align-items: center;
  margin-bottom: 0;
}

.title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.5rem;
  font-style: italic;
  color: var(--color-text);
  margin: 0;
}

.main-text {
  font-family: 'Alex Brush', cursive;
  font-weight: 400;
  text-transform: none;
  font-size: 1.2em;
  color: #78866b;
  display: inline-block;
  padding: 0 5px;
  margin: 0;
}

.sub-text {
  font-size: 1.2rem;
  color: var(--color-text);
  margin-top: 1rem;
  line-height: 1.6;
}

.final-photo-container {
  width: 100%;
  max-width: 400px;
  margin: 2rem auto 0;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  border: 8px solid white;
  background: white;
}

.final-photo {
  width: 100%;
  height: auto;
  display: block;
  transition: transform 0.5s ease;
}

.final-photo:hover {
  transform: scale(1.02);
}

.signature {
  font-family: 'Alex Brush', cursive;
  font-size: 2.5rem;
  color: #78866b;
  margin-top: 2rem;
  margin-bottom: 0;
  line-height: 1.2;
}

@media (max-width: 768px) {
  .final-section {
    padding: 4rem 1rem 0.5rem 1rem;
  }
  
  .signature {
    font-size: 2rem;
  }

  .final-photo-container {
    max-width: 300px;
    margin-top: 1.5rem;
    border-width: 6px;
  }
}
</style>
