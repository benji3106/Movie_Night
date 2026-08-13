<script setup>
defineProps(['movie'])
const emit = defineEmits(['toggle-favorite', 'view-details'])
</script>

<template>
  <div class="bg-slate-900 rounded-2xl overflow-hidden flex flex-col">
    <div class="relative">
      <img :src="movie.image" :alt="movie.title" class="w-full aspect-[2/3] object-cover" />

      <button
        @click="emit('toggle-favorite', movie.id)"
        class="absolute top-3 right-3 w-9 h-9 rounded-full bg-slate-950/70 flex items-center justify-center text-lg"
      >
        <span v-if="movie.favorite" class="text-red-500">♥</span>
        <span v-else class="text-white">♡</span>
      </button>
    </div>

    <div class="p-4 flex flex-col gap-1">
      <h3 class="text-white font-bold">{{ movie.title }}</h3>

      <p class="text-yellow-400 text-sm font-semibold">
        ★ {{ movie.rating }} - {{ movie.year }}
      </p>

      <p class="text-slate-400 text-sm mb-2">{{ movie.category }}</p>

      <button
        @click="emit('view-details', movie)"
        class="w-full text-center px-4 py-2 rounded-full bg-white text-slate-950 text-sm font-bold hover:bg-slate-200 transition-colors"
      >
        Voir le détail
      </button>
    </div>
  </div>
</template>

<style scoped>
</style>