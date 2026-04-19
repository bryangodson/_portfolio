<script setup lang="ts">
import AppNav from "./components/AppNav.vue"
import HeroSection from "./components/HeroSection.vue"
import AboutSection    from './components/AboutSection.vue'
import SkillsSection from "./components/SkillsSection.vue"
import WorkSection from "./components/WorkSection.vue"
import ContactSection from "./components/ContactSection.vue"
import AppFooter from "./components/AppFooter.vue"
import AppCursor from "./components/AppCursor.vue"
</script>

<template>
  <div id="portfolio">
    <AppNav/>
    <AppCursor/>
    <HeroSection/>
    <AboutSection/>
    <SkillsSection/>
    <WorkSection/>
    <ContactSection />
    <AppFooter/>
  </div>
</template>

<style>
#portfolio{
  min-height: 100vh;
}
</style>
