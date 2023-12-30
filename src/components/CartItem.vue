<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number
})

const emit = defineEmits('onRemoveFromCart')
</script>

<template>
  <div class="flex items-center border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-16 h-16 object-contain" :src="imageUrl" alt="Sneaker" />
    <div class="flex flex-col">
      <p class="text-sm">{{ title }}</p>
      <div class="flex justify-between items-center mt-2">
        <b>{{ price }} руб.</b>
        <img
          @click="() => emit('onRemoveFromCart')"
          class="opacity-50 hover:opacity-100 transition cursor-pointer"
          src="/close.svg"
          alt="Close"
        />
      </div>
    </div>
  </div>
</template>
