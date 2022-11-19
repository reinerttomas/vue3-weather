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
        v-if="searchResults"
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
      >
        <p v-if="searchError">Sorry, something went wrong, please try again.</p>
        <p class="py-2" v-if="!searchError && searchResults.length === 0">
          No results match your query, try a different term.
        </p>
        <li
          v-else
          v-for="searchResult in searchResults"
          :key="searchResult.id"
          class="py-2 cursor-pointer"
          @click="previewCity(searchResult)"
        >
          {{ searchResult.place_name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

const query = ref('');
const searchTimeout = ref(null);
const searchResults = ref(null);
const searchError = ref(null);

const mapboxGeocodingUrl = import.meta.env.VITE_MAPBOX_GEOCODING_URL;
const mapboxGeocodingToken = import.meta.env.VITE_MAPBOX_GEOCODING_TOKEN;

const getSearchResults = () => {
  clearTimeout(searchTimeout.value);

  searchTimeout.value = setTimeout(async () => {
    if (query.value !== '') {
      try {
        const result = await axios.get(
          `${mapboxGeocodingUrl}/mapbox.places/${query.value}.json?access_token=${mapboxGeocodingToken}&types=place`
        );

        searchResults.value = result.data.features;
      } catch {
        searchError.value = true;
      }
    } else {
      searchResults.value = null;
    }
  }, 300);
};

const router = useRouter();
const previewCity = (searchResult) => {
  const [city, state] = searchResult.place_name
    .split(',')
    .map((item) => item.trim());

  router.push({
    name: 'city',
    params: { state: state, city: city },
    query: {
      longitude: searchResult.geometry.coordinates[0],
      latitude: searchResult.geometry.coordinates[1],
      preview: true,
    },
  });
};
</script>
