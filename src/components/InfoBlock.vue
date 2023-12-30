<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>

<template>
  <div class="h-full flex flex-col items-center justify-center text-center gap-4">
    <img width="70" height="70" class="mx-auto" :src="imageUrl" alt="Info Icon" />
    <h2 class="text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400">{{ description }}</p>
  </div>
</template>
