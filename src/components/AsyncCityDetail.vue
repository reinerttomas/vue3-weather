<template>
  <div></div>
</template>

<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';

const openWeatherUrl = import.meta.env.VITE_OPEN_WEATHER_URL;
const openWeatherToken = import.meta.env.VITE_OPEN_WEATHER_TOKEN;

const route = useRoute();
const getWeatherData = async () => {
  try {
    const response = await axios.get(
      `${openWeatherUrl}/onecall?lat=${route.query.lat}&lon=${route.query.lng}&appid=${openWeatherToken}&units=imperial`
    );

    // cal current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = response.data.current.dt * 1000 + localOffset;

    response.data.currentTime = utc + 1000 * response.data.timezone_offset;

    // cal hourly weather offset
    response.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset;
      hour.currentTime = utc + 1000 * response.data.timezone_offset;
    });

    return response.data;
  } catch (error) {
    console.log(error);
  }
};

const weatherData = await getWeatherData();
console.log(weatherData);
</script>
