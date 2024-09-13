<script setup>
import { ref, onMounted, computed } from "vue";
import { Star, Loader2 } from "lucide-vue-next";

const TMDB_API_KEY = import.meta.env.VITE_TMDB_API_KEY;

const hoveredRating = ref(0);
const minRating = ref(0);
const selectedGenre = ref("");
const genres = ref([]);
const isSearching = ref(false);
const error = ref(null);
const movieData = ref(null);
const minYear = ref(1900);
const maxYear = ref(new Date().getFullYear());

onMounted(async () => {
  error.value = null;
  try {
    const response = await fetch(
      `https://api.themoviedb.org/3/genre/movie/list?api_key=${TMDB_API_KEY}&language=es-ES`
    );
    if (!response.ok) throw new Error("Error al obtener los géneros");
    const data = await response.json();
    genres.value = data.genres;
  } catch (err) {
    error.value = "Ha ocurrido un problema obteniendo los géneros.";
  }
});

const handleSearch = async () => {
  isSearching.value = true;
  error.value = null;
  if (minYear.value > maxYear.value) minYear.value = maxYear.value;
  try {
    const pages_response = await fetch(
      `https://api.themoviedb.org/3/discover/movie?api_key=${TMDB_API_KEY}&language=es-ES&with_genres=${
        selectedGenre.value
      }&vote_average.gte=${
        (minRating.value - 1) * 2
      }&vote_count.gte=100&primary_release_date.gte=${
        minYear.value
      }-01-01&primary_release_date.lte=${maxYear.value}-12-31`
    );
    const total_pages = await pages_response.json();
    const randomPage =
      Math.floor(Math.random() * Math.min(total_pages.total_pages, 499)) + 1;
    const films = await fetch(
      `https://api.themoviedb.org/3/discover/movie?api_key=${TMDB_API_KEY}&language=es-ES&with_genres=${
        selectedGenre.value
      }&page=${randomPage}&vote_average.gte=${
        (minRating.value - 1) * 2
      }&vote_count.gte=100&primary_release_date.gte=${
        minYear.value
      }-01-01&primary_release_date.lte=${maxYear.value}-12-31`
    );
    const filmData = await films.json();
    movieData.value =
      filmData.results[Math.floor(Math.random() * filmData.results.length)];
  } catch (err) {
    error.value =
      "Error obteniendo la información de la película. Inténtelo de nuevo más tarde";
  } finally {
    isSearching.value = false;
  }
};

const handleStarHover = (star) => {
  hoveredRating.value = star;
};

const isActive = computed(() => (star) => {
  const rating = hoveredRating.value || minRating.value;
  return rating >= star;
});

const selectStarClick = (star) => {
  minRating.value = star;
};
</script>

<template>
  <div
    class="min-h-screen bg-gradient-to-br from-teal-400 to-blue-500 py-12 px-4 sm:px-6 lg:px-8"
  >
    <div class="max-w-3xl mx-auto">
      <h1 class="text-4xl font-extrabold text-white text-center mb-12">
        Generador automático de películas
      </h1>

      <div class="bg-white rounded-lg shadow-xl p-8 space-y-8">
        <div>
          <h2
            class="text-2xl text-center font-semibold text-gray-800 mb-4 md:text-start"
          >
            Selecciona un género
          </h2>
          <select
            v-model="selectedGenre"
            class="w-full p-3 bg-gray-100 rounded-lg text-gray-800 focus:outline-none text-center md:text-start"
          >
            <option value="">Cualquier género</option>
            <option v-for="genre in genres" :key="genre.id" :value="genre.id">
              {{ genre.name }}
            </option>
          </select>
        </div>

        <div>
          <h2
            class="text-2xl font-semibold text-gray-800 text-center mb-4 md:text-start"
          >
            Puntuación mínima
          </h2>
          <div class="flex justify-center" @mouseleave="hoveredRating = 0">
            <Star
              v-for="star in 5"
              :key="star"
              :fill="isActive(star) ? 'gold' : 'none'"
              :stroke="isActive(star) ? 'gold' : 'currentColor'"
              size="48"
              class="text-gray-400 cursor-pointer transition-all duration-200 hover:scale-110"
              @click="selectStarClick(star)"
              @mouseenter="handleStarHover(star)"
            />
          </div>
        </div>

        <div class="flex space-x-4">
          <div class="flex-1">
            <h2
              class="text-2xl font-semibold text-gray-800 mb-4 text-center md:text-start"
            >
              Año mínimo
            </h2>
            <input
              v-model="minYear"
              type="number"
              min="1900"
              :max="maxYear"
              class="w-full p-3 bg-gray-100 rounded-lg text-gray-800 focus:outline-none"
            />
          </div>
          <div class="flex-1">
            <h2
              class="text-2xl font-semibold text-gray-800 mb-4 text-center md:text-start"
            >
              Año máximo
            </h2>
            <input
              v-model="maxYear"
              type="number"
              :min="minYear"
              :max="new Date().getFullYear()"
              class="w-full p-3 bg-gray-100 rounded-lg text-gray-800 focus:outline-none"
            />
          </div>
        </div>

        <button
          class="w-full bg-teal-600 text-white p-4 rounded-lg font-semibold hover:bg-teal-700 focus:outline-none transition-all duration-200"
          :disabled="isSearching"
          @click="handleSearch"
        >
          <span v-if="!isSearching">Buscar película</span>
          <Loader2 v-else class="animate-spin mx-auto" />
        </button>
      </div>

      <div
        v-if="movieData"
        class="mt-12 bg-white rounded-lg shadow-xl overflow-hidden"
      >
        <div class="md:flex">
          <div class="md:w-2/5">
            <img
              :src="`https://image.tmdb.org/t/p/w500${movieData.poster_path}`"
              :alt="movieData.title"
              class="w-full h-full object-cover"
            />
          </div>
          <div class="p-8 md:w-3/5 flex flex-col justify-between">
            <div>
              <h3 class="text-3xl font-bold mb-4 text-gray-800">
                {{ movieData.title }}
              </h3>
              <p class="text-gray-600 leading-relaxed mb-6">
                {{ movieData.overview }}
              </p>
            </div>
            <div class="flex justify-between items-center">
              <span class="text-sm text-gray-500">
                Fecha de estreno:
                {{ new Date(movieData.release_date).toLocaleDateString() }}
              </span>
              <div
                class="flex items-center bg-yellow-400 text-gray-900 font-bold px-3 py-1 rounded-full"
              >
                <Star class="w-4 h-4 mr-1" />
                {{ movieData.vote_average.toFixed(1) }}
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        v-if="error"
        class="mt-8 bg-red-100 border-l-4 border-red-500 text-red-700 p-4 rounded"
        role="alert"
      >
        <p class="font-bold">Error</p>
        <p>{{ error }}</p>
      </div>
    </div>
  </div>
</template>
