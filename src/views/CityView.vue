<template>
  <div>
    <Suspense>
      <AsyncCityDetail />
      <template #fallback>
        <CityDetailSkeleton />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncCityDetail from '@/components/AsyncCityDetail.vue';
import CityDetailSkeleton from '@/components/CityDetailSkeleton.vue';
</script>
