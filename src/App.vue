<script setup>
import Navbar from './components/Navbar.vue'
import HeroSection from './components/HeroSection.vue'
import CardGrid from './components/CardGrid.vue'
import AnnouncementBar from './components/AnnouncementBar.vue'
import ProductCarousel from './components/ProductCarousel.vue'
</script>

<template>
  <AnnouncementBar />
  <Navbar />
  <HeroSection />
  <CardGrid />
  <ProductCarousel />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
