<script setup>
import { ref, onMounted } from 'vue'
import LoadingSpinner from './LoadingSpinner.vue'
import MovieCard from './MovieCard.vue'

const movies = ref([])
const isLoading = ref(true)
const errorMessage = ref(null)

async function loadMovies() {
  isLoading.value = true
  errorMessage.value = null

  try {
    await new Promise((resolve) => setTimeout(resolve, 2000))

    const response = await fetch('/data/movies.json')
    if (!response.ok) {
      throw new Error('Impossible de charger les films (statut ' + response.status + ')')
    }

    const data = await response.json()
    movies.value = data
  } catch (error) {
    console.error(error)
    errorMessage.value = 'Une erreur est survenue.'
  } finally {
    isLoading.value = false
  }
}

onMounted(() => {
  loadMovies()
})
</script>

<template>
  <div class="mt-8">
    <div v-if="isLoading" class="min-h-[400px] flex items-center justify-center">
      <LoadingSpinner />
    </div>

    <div v-else-if="errorMessage" class="min-h-[400px] flex items-center justify-center">
      <div class="bg-slate-900 rounded-2xl px-10 py-8 flex flex-col items-center gap-4 text-center">
        <p class="text-white font-bold text-lg">Impossible de charger les films</p>
        <p class="text-slate-400 text-sm -mt-2">Une erreur est survenue pendant le chargement.</p>
        <button
          @click="loadMovies"
          class="px-5 py-2 rounded-full bg-red-500 text-white text-sm font-medium hover:bg-red-600 transition-colors"
        >
          Réessayer
        </button>
      </div>
    </div>

    <div v-else class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
      <MovieCard v-for="movie in movies" :key="movie.id" :movie="movie" />
    </div>
  </div>
</template>

<style scoped>
</style>