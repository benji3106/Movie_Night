<script setup>
defineProps(['movie'])
</script>

<template>
  <div class="bg-slate-900 rounded-2xl overflow-hidden flex flex-col">
    <img :src="movie.image" :alt="movie.title" class="w-full h-64 object-cover" />

    <div class="p-4 flex flex-col gap-2">
      <div class="flex items-start justify-between gap-2">
        <h3 class="text-white font-bold">{{ movie.title }}</h3>
        <span class="text-yellow-400 text-sm font-semibold whitespace-nowrap">★ {{ movie.rating }}</span>
      </div>

      <p class="text-slate-400 text-sm">{{ movie.category }} · {{ movie.year }} · {{ movie.duration }}</p>

      <p class="text-slate-300 text-sm line-clamp-2">{{ movie.description }}</p>
    </div>
  </div>
</template>

<style scoped>
</style>