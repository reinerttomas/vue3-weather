<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-weather-primary">
    <TheNavigation />
    <RouterView v-slot="{ Component }">
      <Transition name="page" mode="out-in">
        <component :is="Component"></component>
      </Transition>
    </RouterView>
  </div>
</template>

<script setup>
import { RouterView } from 'vue-router';
import TheNavigation from '@/components/TheNavigation.vue';
</script>

<style>
.page-enter-active,
page-leave-active {
  transition: 600ms ease all;
}
.page-enter-from,
.page-leave-to {
  opacity: 0;
}
</style>
