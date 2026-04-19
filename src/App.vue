<script setup lang="ts">
import AppNav from "./components/AppNav.vue"
import HeroSection from "./components/HeroSection.vue"
import AboutSection    from './components/AboutSection.vue'
import SkillsSection from "./components/SkillsSection.vue"
import WorkSection from "./components/WorkSection.vue"
import ContactSection from "./components/ContactSection.vue"
</script>

<template>
  <div id="portfolio">
    <AppNav/>
    <HeroSection/>
    <AboutSection/>
    <SkillsSection/>
    <WorkSection/>
    <ContactSection />
  </div>
</template>

<style>
#portfolio{
  min-height: 100vh;
}
</style>
