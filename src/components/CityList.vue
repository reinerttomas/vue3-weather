<template>
  <div v-for="city in cities" :key="city.id">
    <CityCard :city="city" @click="goToCityView(city)" />
  </div>

  <p v-if="cities.length === 0">
    No locations added. To start tracking a location, search in the field above.
  </p>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import CityCard from '@/components/CityCard.vue';
import { useRouter } from 'vue-router';

const openWeatherUrl = import.meta.env.VITE_OPEN_WEATHER_URL;
const openWeatherToken = import.meta.env.VITE_OPEN_WEATHER_TOKEN;

const cities = ref([]);

const getCities = async () => {
  if (localStorage.getItem('cities')) {
    cities.value = JSON.parse(localStorage.getItem('cities'));
  }

  const request = [];

  cities.value.forEach((city) => {
    request.push(
      axios.get(
        `${openWeatherUrl}/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=${openWeatherToken}&units=imperial`
      )
    );
  });

  const weatherData = await Promise.all(request);

  weatherData.forEach((value, index) => {
    cities.value[index].weather = value.data;
  });
};

await getCities();

const router = useRouter();
const goToCityView = (city) => {
  router.push({
    name: 'city',
    params: { state: city.state, city: city.city },
    query: { id: city.id, lat: city.coords.lat, lng: city.coords.lng },
  });
};
</script>
