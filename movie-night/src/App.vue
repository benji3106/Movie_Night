<script setup>
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import MovieList from './components/MovieList.vue'
</script>

<template>
  <div class="min-h-screen flex flex-col bg-slate-950">
    <AppHeader />

    <main class="flex-1 px-6 py-10">
      <h2 class="text-2xl font-bold text-white">Que voulez-vous regarder ce soir ?</h2>
      <p class="text-slate-400 mt-1">Parcourez, filtrez et gardez vos films préférés.</p>

      <MovieList />
    </main>

    <AppFooter />
  </div>
</template>

<style scoped>
</style>