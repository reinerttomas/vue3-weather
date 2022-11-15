<template>
  <div class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        v-model="query"
        @input="getSearchResults"
        placeholder="Search for city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-sm"
      />
      <ul
        v-if="queryResults"
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
      >
        <li
          v-for="queryResult in queryResults"
          :key="queryResult.id"
          class="py-2 cursor-pointer"
        >
          {{ queryResult.place_name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const query = ref('');
const queryTimeout = ref(null);
const queryResults = ref(null);

const mapboxGeocodingUrl = import.meta.env.VITE_MAPBOX_GEOCODING_URL;
const mapboxGeocodingToken = import.meta.env.VITE_MAPBOX_GEOCODING_TOKEN;

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);

  queryTimeout.value = setTimeout(async () => {
    if (query.value !== '') {
      const result = await axios.get(
        `${mapboxGeocodingUrl}/mapbox.places/${query.value}.json?access_token=${mapboxGeocodingToken}&types=place`
      );

      queryResults.value = result.data.features;
      console.log(queryResults.value);
    } else {
      queryResults.value = null;
    }
  }, 300);
};
</script>
