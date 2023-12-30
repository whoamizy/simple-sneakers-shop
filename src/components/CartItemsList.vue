<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'
import InfoBlock from './InfoBlock.vue'

const cartItems = inject('cartItems')
const removeFromCart = inject('removeFromCart')

console.log(cartItems)
</script>

<template>
  <div class="flex-1 flex flex-col gap-5 overflow-auto px-8" v-auto-animate>
    <InfoBlock
      v-if="!cartItems.length"
      title="Корзина пустая"
      description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
      imageUrl="/package-icon.png"
    />
    <CartItem
      v-for="item in cartItems"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      @on-remove-from-cart="() => removeFromCart(item)"
    />
  </div>
</template>
