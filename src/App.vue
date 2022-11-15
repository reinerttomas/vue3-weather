<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-weather-primary">
    <TheNavigation />
    <RouterView />
  </div>
</template>

<script setup>
import { RouterView } from 'vue-router';
import TheNavigation from '@/components/TheNavigation.vue';
</script>
