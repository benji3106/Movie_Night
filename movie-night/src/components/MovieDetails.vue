<script setup>
defineProps(['movie'])
const emit = defineEmits(['close', 'toggle-favorite'])
</script>

<template>
  <div
    class="fixed inset-0 bg-slate-950/80 flex items-center justify-center px-4 z-50"
    @click.self="emit('close')"
  >
    <div class="bg-slate-900 rounded-2xl overflow-hidden w-full max-w-3xl max-h-[90vh] overflow-y-auto relative flex flex-col sm:flex-row">
      <button
        @click="emit('close')"
        class="absolute top-4 right-4 w-9 h-9 rounded-full bg-slate-950/70 text-white flex items-center justify-center hover:bg-slate-950 transition-colors z-10"
      >
        ✕
      </button>

      <img
        :src="movie.image"
        :alt="movie.title"
        class="w-full sm:w-2/5 h-64 sm:h-auto object-cover flex-shrink-0"
      />

      <div class="p-6 sm:p-8 flex flex-col gap-3">
        <p class="text-red-500 text-xs font-bold tracking-widest uppercase">
          {{ movie.category }}
        </p>

        <h2 class="text-white text-2xl sm:text-3xl font-bold">{{ movie.title }}</h2>

        <p class="text-slate-400 text-sm flex items-center gap-2">
          <span>{{ movie.year }}</span>
          <span>·</span>
          <span>{{ movie.duration }}</span>
          <span>·</span>
          <span class="text-yellow-400 font-semibold">★ {{ movie.rating }}</span>
        </p>

        <p class="text-slate-300 text-sm mt-2">{{ movie.description }}</p>

        <div class="flex gap-3 mt-4">
          <button
            @click="emit('toggle-favorite', movie.id)"
            class="px-5 py-2 rounded-full bg-red-500 text-white text-sm font-bold hover:bg-red-600 transition-colors"
          >
            {{ movie.favorite ? 'Retirer des favoris' : 'Ajouter aux favoris' }}
          </button>
          <button
            @click="emit('close')"
            class="px-5 py-2 rounded-full bg-white text-slate-950 text-sm font-bold hover:bg-slate-200 transition-colors"
          >
            Fermer
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>