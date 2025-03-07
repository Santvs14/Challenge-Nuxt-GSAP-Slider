<template>
  <div class="slider">
    <div class="slider-container" ref="sliderContainer">
      <div
        v-for="(slide, index) in slides"
        :key="index"
        class="slide"
        :class="{ active: index === activeIndex }"
      >
        <img :src="slide.image" alt="Slide Image" class="slide-image" />
        <h1 ref="titles" class="slide-title">{{ slide.text }}</h1>
      </div>
    </div>

    <!-- Botones de navegación -->
    <button class="prev" @click="prevSlide">❮</button>
    <button class="next" @click="nextSlide">❯</button>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import gsap from "gsap";

const slides = ref([
  {
    image: "https://source.unsplash.com/800x500/?nature",
    text: "Bienvenidos a la Naturaleza",
  },
  {
    image: "https://source.unsplash.com/800x500/?city",
    text: "Explora la Ciudad",
  },
  {
    image: "https://source.unsplash.com/800x500/?ocean",
    text: "La Belleza del Mar",
  },
]);

const activeIndex = ref(0);
const titles = ref([]);
const sliderContainer = ref(null);

const animateSlide = () => {
  gsap.to(sliderContainer.value, {
    x: -activeIndex.value * 800, // Desplazamiento en base al índice
    duration: 1,
    ease: "power3.inOut",
  });

  gsap.fromTo(
    titles.value[activeIndex.value],
    { opacity: 0, y: 50 },
    { opacity: 1, y: 0, duration: 1, ease: "power3.out" }
  );
};

// Observar cambios en el slide activo y animar
watch(activeIndex, () => {
  animateSlide();
});

const nextSlide = () => {
  activeIndex.value = (activeIndex.value + 1) % slides.value.length;
};

const prevSlide = () => {
  activeIndex.value =
    (activeIndex.value - 1 + slides.value.length) % slides.value.length;
};

onMounted(() => {
  animateSlide(); // Inicia con la primera animación
});
</script>

<style lang="scss" scoped>
/* Estilos generales */
body {
  margin: 0;
  padding: 0;
  font-family: "Arial", sans-serif;
  background-color: #1e1e1e;
}

/* Slider */
.slider {
  position: relative;
  width: 800px;
  height: 500px;
  margin: auto;
  overflow: hidden;
}

/* Contenedor de slides */
.slider-container {
  display: flex;
  width: 2400px; /* 800px * 3 imágenes */
  transition: transform 0.5s ease-in-out;
}

/* Slide */
.slide {
  width: 800px;
  height: 100%;
  flex-shrink: 0;
  position: relative;
}

/* Imágenes */
.slide-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 10px;
}

/* Texto animado */
.slide-title {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 2.5rem;
  font-weight: bold;
  color: #fff;
  text-shadow: 2px 2px 10px rgba(0, 0, 0, 0.5);
  opacity: 0;
}

/* Botones de navegación */
.prev,
.next {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 2rem;
  cursor: pointer;
  border-radius: 5px;
}

.prev {
  left: 10px;
}

.next {
  right: 10px;
}

.prev:hover,
.next:hover {
  background: rgba(0, 0, 0, 0.8);
}
</style>
