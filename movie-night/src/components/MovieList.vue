<script setup>
import { ref, computed } from 'vue'
import LoadingSpinner from './LoadingSpinner.vue'
import MovieCard from './MovieCard.vue'

const props = defineProps({
  movies: { type: Array, required: true },
  isLoading: { type: Boolean, default: false },
  errorMessage: { type: String, default: null },
  showFilters: { type: Boolean, default: true },
  isFavoritesView: { type: Boolean, default: false }
})

const emit = defineEmits(['reload', 'toggle-favorite', 'go-to-films', 'view-details'])

const search = ref('')
const selectedCategory = ref(null)

const categories = computed(() => {
  return [...new Set(props.movies.map((movie) => movie.category))]
})

function toggleCategory(category) {
  selectedCategory.value = selectedCategory.value === category ? null : category
}

function resetFilters() {
  search.value = ''
  selectedCategory.value = null
}

const filteredMovies = computed(() => {
  return props.movies.filter((movie) => {
    const matchesSearch = movie.title.toLowerCase().includes(search.value.toLowerCase())
    const matchesCategory = !selectedCategory.value || movie.category === selectedCategory.value
    return matchesSearch && matchesCategory
  })
})
</script>

<template>
  <div class="mt-8">
    <template v-if="showFilters && !isLoading && !errorMessage">
      <input
        v-model="search"
        type="text"
        placeholder="Rechercher un film..."
        class="w-full max-w-sm px-4 py-2 rounded-full bg-slate-800 text-white placeholder-slate-500 mb-4 focus:outline-none focus:ring-2 focus:ring-red-500"
      />
      <div class="flex flex-wrap gap-2 mb-6">
        <button
          v-for="cat in categories"
          :key="cat"
          @click="toggleCategory(cat)"
          :class="selectedCategory === cat
            ? 'bg-red-500 text-white'
            : 'bg-slate-800 text-slate-300 hover:bg-slate-700'"
          class="px-4 py-2 rounded-full text-sm font-medium transition-colors"
        >
          {{ cat }}
        </button>
      </div>
    </template>

    <div v-if="isLoading" class="min-h-[400px] flex items-center justify-center">
      <LoadingSpinner />
    </div>

    <div v-else-if="errorMessage" class="min-h-[400px] flex items-center justify-center">
      <div class="bg-slate-900 rounded-2xl px-6 sm:px-10 py-8 flex flex-col items-center gap-4 text-center">
        <p class="text-white font-bold text-lg">Impossible de charger les films</p>
        <p class="text-slate-400 text-sm -mt-2">Une erreur est survenue pendant le chargement.</p>
        <button
          @click="emit('reload')"
          class="px-5 py-2 rounded-full bg-red-500 text-white text-sm font-medium hover:bg-red-600 transition-colors"
        >
          Réessayer
        </button>
      </div>
    </div>

    <div v-else-if="filteredMovies.length === 0 && isFavoritesView" class="min-h-[400px] flex items-center justify-center px-4">
      <div class="bg-slate-900 rounded-2xl px-6 sm:px-10 py-8 flex flex-col items-center gap-4 text-center max-w-sm w-full">
        <p class="text-white font-bold text-lg">Aucun favori pour le moment</p>
        <p class="text-slate-400 text-sm -mt-2">Ajoutez des films en cliquant sur le cœur.</p>
        <button
          @click="emit('go-to-films')"
          class="px-5 py-2 rounded-full bg-red-500 text-white text-sm font-medium hover:bg-red-600 transition-colors"
        >
          Retour aux films
        </button>
      </div>
    </div>

    <div v-else-if="filteredMovies.length === 0" class="min-h-[400px] flex items-center justify-center px-4">
      <div class="bg-slate-900 rounded-2xl px-6 sm:px-10 py-8 flex flex-col items-center gap-4 text-center max-w-sm w-full">
        <p class="text-white font-bold text-lg">Aucun film trouvé</p>
        <p class="text-slate-400 text-sm -mt-2">Essayez avec d'autres mots-clés ou une autre catégorie.</p>
        <button
          @click="resetFilters"
          class="px-5 py-2 rounded-full bg-red-500 text-white text-sm font-medium hover:bg-red-600 transition-colors"
        >
          Réinitialiser les filtres
        </button>
      </div>
    </div>

    <div v-else class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
      <MovieCard
        v-for="movie in filteredMovies"
        :key="movie.id"
        :movie="movie"
        @toggle-favorite="emit('toggle-favorite', $event)"
        @view-details="emit('view-details', $event)"
      />
    </div>
  </div>
</template>

<style scoped>
</style>