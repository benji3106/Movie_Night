<script setup>
defineProps(['currentView', 'favoriteCount'])
const emit = defineEmits(['navigate'])
</script>

<template>
  <header class="bg-slate-950 border-b border-slate-800 px-6 py-4 flex items-center justify-between">
    <div class="flex items-center gap-3">
      <div class="w-9 h-9 rounded-xl bg-red-500 flex items-center justify-center font-bold text-white">M</div>
      <h1 class="text-lg font-bold tracking-wide text-white">MOVIE NIGHT</h1>
    </div>

    <nav class="flex gap-3">
      <button
        @click="emit('navigate', 'films')"
        :class="currentView === 'films' ? 'bg-red-500 text-white' : 'bg-slate-800 text-white hover:bg-slate-700'"
        class="px-4 py-2 rounded-full text-sm font-medium transition-colors"
      >
        Films
      </button>
      <button
        @click="emit('navigate', 'favorites')"
        :class="currentView === 'favorites' ? 'bg-red-500 text-white' : 'bg-slate-800 text-white hover:bg-slate-700'"
        class="px-4 py-2 rounded-full text-sm font-medium flex items-center gap-2 transition-colors"
      >
        <span>♥</span> Favoris <span class="bg-slate-600 rounded-full px-2 text-xs">{{ favoriteCount }}</span>
      </button>
    </nav>
  </header>
</template>

<style scoped>
</style>