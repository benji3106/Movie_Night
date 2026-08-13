<script setup>
import { LoaderCircle } from 'lucide-vue-next'
</script>

<template>
  <div class="flex flex-col items-center justify-center gap-4">
    <LoaderCircle class="w-12 h-12 text-red-500 animate-spin" />

    <div class="text-center">
      <p class="text-white font-semibold">Chargement des films...</p>
      <p class="text-slate-400 text-sm mt-1">Préparation de votre sélection</p>
    </div>
  </div>
</template>

<style scoped>
</style>