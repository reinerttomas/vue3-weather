<template>
  <div>
    <Suspense>
      <AsyncCityDetail />
      <template #fallback>
        <p>Loading...</p>
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncCityDetail from '@/components/AsyncCityDetail.vue';
</script>
