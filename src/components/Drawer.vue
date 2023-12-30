<script setup>
import CartItemsList from './CartItemsList.vue'

defineProps({
  totalPrice: Number
})

const emit = defineEmits(['closeDrawer', 'createOrder'])
</script>

<template>
  <div
    class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"
    @click="() => emit('closeDrawer')"
  ></div>
  <div class="fixed top-0 right-0 bg-white py-8 w-96 h-full z-20 flex flex-col">
    <h2 class="text-2xl font-bold mb-4 px-8">Корзина</h2>
    <CartItemsList />
    <div class="px-8 flex flex-col gap-5 mt-4">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed mb-1"></div>
        <b>{{ totalPrice }} руб.</b>
      </div>
      <button
        @click="() => emit('createOrder')"
        :disabled="!totalPrice"
        class="cursor-pointer bg-lime-400 w-full rounded-xl py-3 text-white hover:bg-lime-500 disabled:bg-slate-300 transition"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
