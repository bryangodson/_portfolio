<script setup lang="ts">
import AppNav from "./components/AppNav.vue"
import HeroSection from "./components/HeroSection.vue"
import AboutSection    from './components/AboutSection.vue'

</script>

<template>
  <div id="portfolio">
    <AppNav/>
    <HeroSection/>
    <AboutSection/>
  </div>
</template>

<style>
#portfolio{
  min-height: 100vh;
}
</style>
