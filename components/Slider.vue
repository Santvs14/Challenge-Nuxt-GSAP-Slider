<template>
  <div class="slider">
    <audio ref="backgroundMusic" loop>
      <source src="https://www.bensound.com/bensound-music/bensound-hey.mp3" type="audio/mpeg" />
      Tu navegador no soporta el elemento de audio.
    </audio>

    <div class="slider-background" :style="{ backgroundImage: `url(${currentSlide.media?.src})` }"></div>

    <div class="slider-container">
      <div
        v-for="(slide, index) in slideData"
        :key="index"
        class="slide"
        :class="{
          active: index === currentIndex,
          prev: index === getPrevIndex(),
          next: index === getNextIndex(),
        }"
        ref="slides"
        @click="handleSlideClick(index)"
      >
        <div class="slide-mask">
          <h2 v-if="index === currentIndex" ref="title" class="slide-title">{{ slide.title }}</h2>
          <component
            :is="slide.media?.type === 'video' ? 'video' : 'img'"
            v-if="slide.media?.src"
            :src="slide.media.src"
            class="slide-media"
            autoplay
            loop
            muted
          />
        </div>
      </div>
    </div>

    <div class="title-list">
      <ul>
        <li
          v-for="(slide, index) in slideData"
          :key="index"
          :class="{ activeTitle: index === currentIndex }"
        >
          {{ slide.title }}
        </li>
      </ul>
    </div>

    <button @click="nextSlide" class="next-button">›</button>
    <button @click="toggleMusic" class="music-button">
      🎵 {{ isMusicPlaying ? "Pausar" : "Reproducir" }}
    </button>
  </div>
</template>

<script>
import gsap from "gsap";

export default {
  props: {
    slides: {
      type: Array,
      required: true,
    },
    autoplay: {
      type: Boolean,
      default: true,
    },
    autoplayDelay: {
      type: Number,
      default: 3000,
    },
  },
  data() {
    return {
      currentIndex: 0,
      slideData: [],
      autoplayInterval: null,
      isMusicPlaying: false,
    };
  },
  computed: {
    currentSlide() {
      return this.slideData[this.currentIndex] || { media: { src: "", type: "image" } };
    },
  },
  mounted() {
    this.initSlides();
    if (this.autoplay) {
      this.startAutoplay();
    }
    this.playMusic();
  },
  methods: {
    initSlides() {
      this.slideData = [...this.slides];
      while (this.slideData.length < 3) {
        this.slideData = [...this.slideData, ...this.slides];
      }
    },
    nextSlide() {
      const prevIndex = this.getPrevIndex();
      const nextIndex = this.getNextIndex();
      const currentSlide = this.$refs.slides[this.currentIndex];
      const nextSlide = this.$refs.slides[nextIndex];
      const prevSlide = this.$refs.slides[prevIndex];

      if (currentSlide && nextSlide && prevSlide) {
        // Desaparece el slide activo con desvanecimiento
        gsap.to(currentSlide, { opacity: 0, scale: 0.5, x: "-100%", duration: 1 });

        // Aparece el siguiente slide con desvanecimiento y movimiento
        gsap.to(nextSlide, { opacity: 1, scale: 1, x: "0%", duration: 1 });

        // Desaparece el slide previo con desvanecimiento
        gsap.to(prevSlide, { opacity: 0, scale: 0.5, x: "-200%", duration: 1 });
      }

      // Animación para los títulos activos
      this.animateTitles();

      setTimeout(() => {
        // Después de pasar todos los slides, vuelve al primer slide
        this.currentIndex = nextIndex;
        if (this.currentIndex === this.slideData.length) {
          this.currentIndex = 0; // Reiniciar desde el principio
        }
      }, 1000);
    },

    prevSlide() {
      const prevIndex = this.getPrevIndex();
      this.currentIndex = prevIndex;
    },
    getPrevIndex() {
      return (this.currentIndex - 1 + this.slideData.length) % this.slideData.length;
    },
    getNextIndex() {
      return (this.currentIndex + 1) % this.slideData.length;  // Ciclo cuando llega al final
    },
    handleSlideClick(index) {
      if (index === this.getNextIndex()) {
        this.nextSlide();
      } else if (index === this.getPrevIndex()) {
        this.prevSlide();
      }
    },
    startAutoplay() {
      this.autoplayInterval = setInterval(this.nextSlide, this.autoplayDelay);
    },
    stopAutoplay() {
      clearInterval(this.autoplayInterval);
    },
    playMusic() {
      const audio = this.$refs.backgroundMusic;
      if (audio) {
        audio.volume = 0.1;
        audio.play();
        this.isMusicPlaying = true;
      }
    },
    toggleMusic() {
      const audio = this.$refs.backgroundMusic;
      if (audio.paused) {
        audio.play();
        this.isMusicPlaying = true;
      } else {
        audio.pause();
        this.isMusicPlaying = false;
      }
    },
    animateTitles() {
      // Animación para cada título de los slides
      this.slideData.forEach((_, index) => {
        const title = this.$refs.title[index];
        if (title) {
          gsap.fromTo(title, { opacity: 0, y: 50 }, { opacity: 1, y: 0, duration: 1 });
        }
      });
    },
    beforeDestroy() {
      this.stopAutoplay();
    },
  },
};
</script>

<style lang="scss" scoped>
.slider {
  position: relative;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  background-color: black;
}

.slider-background {
  position: absolute;
  width: 60%;
  height: 48%;
  background-size: cover;
  background-position: center;
  transition: background-image 0.5s ease-in-out;
}

.slider-container {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  width: 100%;
  height: 100%;
}

.slide-title {
  position: absolute;
  bottom: 12%;
  left: 50%;
  transform: translateX(-55%);
  color: white;
  font-size: 1.3rem;
}

.slide {
  position: absolute;
  transition: transform 1s ease-in-out, opacity 1s ease-in-out;
  opacity: 0;
  width: 20%;
  height: 20%;
  display: flex;
  align-items: center;
  justify-content: center;

  .slide-mask {
    overflow: hidden;
    width: 100%;
    height: 100%;
  }

  &.active {
    opacity: 1;
    transform: translateX(0);
    z-index: 10;
    width: 20%;
    height: 40%;
    scale: 1;
  }

  &.prev {
    width: 20%;
    height: 20%;
    opacity: 0.4;
    scale: 0.7;
    transform: translateX(-150%);
  }

  &.next {
    width: 20%;
    height: 20%;
    opacity: 0.7;
    scale: 0.8;
    transform: translateX(150%);
  }
}

.title-list {
  position: absolute;
  bottom: 20px;
  left: 20px;
}

.title-list li {
  font-size: 1rem;
  color: white;
  opacity: 0.6;
  margin-bottom: 5px;
  transition: opacity 0.3s;
}

.title-list .activeTitle {
  font-weight: bold;
  opacity: 1;
  color: #ffeb3b;
}

button {
  position: absolute;
  top: 90%;
  right: 20px;
  background: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  font-size: 1.3rem;
  padding: 7px;
  cursor: pointer;
  transform: translateY(-50%);
}

.music-button {
  position: absolute;
  bottom: 20px;
  right: 20px;
  background: gray;
  color: white;
  border: none;
  font-size: 1rem;
  padding: 5px;
  cursor: pointer;
  border-radius: 8px;
}
</style>
