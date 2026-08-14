<script setup>
import { ref, computed, onMounted } from 'vue'
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import MovieList from './components/MovieList.vue'
import MovieDetails from './components/MovieDetails.vue'

const movies = ref([])
const isLoading = ref(true)
const errorMessage = ref(null)
const currentView = ref('films')
const selectedMovie = ref(null)

async function loadMovies() {
  isLoading.value = true
  errorMessage.value = null
  try {
    await new Promise((resolve) => setTimeout(resolve, 2000))
    const response = await fetch('./data/movies.json')
    if (!response.ok) {
      throw new Error('Impossible de charger les films (statut ' + response.status + ')')
    }
    movies.value = await response.json()
  } catch (error) {
    console.error(error)
    errorMessage.value = 'Une erreur est survenue.'
  } finally {
    isLoading.value = false
  }
}

function toggleFavorite(movieId) {
  const movie = movies.value.find((m) => m.id === movieId)
  if (movie) {
    movie.favorite = !movie.favorite
  }
}

const favoriteMovies = computed(() => movies.value.filter((movie) => movie.favorite))
const favoriteCount = computed(() => favoriteMovies.value.length)

function handleNavigate(view) {
  currentView.value = view
}

function openDetails(movie) {
  selectedMovie.value = movie
}

function closeDetails() {
  selectedMovie.value = null
}

onMounted(() => {
  loadMovies()
})
</script>

<template>
  <div class="min-h-screen flex flex-col bg-slate-950">
    <AppHeader
      :current-view="currentView"
      :favorite-count="favoriteCount"
      @navigate="handleNavigate"
    />

    <main class="flex-1 px-6 py-10">
      <template v-if="currentView === 'films'">
        <h2 class="text-2xl font-bold text-white">Que voulez-vous regarder ce soir ?</h2>
        <p class="text-slate-400 mt-1">Parcourez, filtrez et gardez vos films préférés.</p>
      </template>
      <template v-else>
        <h2 class="text-2xl font-bold text-white">Vos favoris</h2>
        <p class="text-slate-400 mt-1">Retrouvez ici tous les films que vous avez aimés.</p>
      </template>

      <MovieList
        :movies="currentView === 'films' ? movies : favoriteMovies"
        :is-loading="isLoading"
        :error-message="errorMessage"
        :show-filters="currentView === 'films'"
        :is-favorites-view="currentView === 'favorites'"
        @reload="loadMovies"
        @toggle-favorite="toggleFavorite"
        @go-to-films="handleNavigate('films')"
        @view-details="openDetails"
      />
    </main>

    <AppFooter />

    <MovieDetails
      v-if="selectedMovie"
      :movie="selectedMovie"
      @close="closeDetails"
      @toggle-favorite="toggleFavorite"
    />
  </div>
</template>

<style scoped>
</style>