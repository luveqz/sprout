<script setup lang="ts">
import { inject } from '@vercel/analytics'
import { RouterView } from 'vue-router'
import TheNavbar from '@/components/TheNavbar.vue'
import { onMounted } from 'vue'

// Setup Vercel Analytics.
onMounted(inject)
</script>

<template>
  <div class="main-container">
    <TheNavbar />
    <RouterView class="router-view" />
  </div>
</template>

<style lang="scss">
@import 'normalize.css';
@import '@/assets/css/main.scss';

.main-container {
  width: 100%;
  position: absolute;
  min-height: 100%;
  display: flex;
  flex-direction: column;
}

.router-view {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
}
</style>
