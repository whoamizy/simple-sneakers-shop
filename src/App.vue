<script setup>
import { computed, provide, ref, watch } from 'vue'
import axios from 'axios'
import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

const cartItems = ref([])
const isDrawerOpen = ref(false)

const totalPrice = computed(() => cartItems.value.reduce((acc, cur) => (acc += cur.price), 0))

const openDrawer = () => {
  isDrawerOpen.value = true
}

const closeDrawer = () => {
  isDrawerOpen.value = false
}

const addToCart = (item) => {
  cartItems.value.push(item)
  item.isAdded = true
}
const removeFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}

const createOrder = async () => {
  try {
    const { data } = await axios.post('https://d585671a9582f90c.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: totalPrice.value
    })
    cartItems.value.map((item) => ({
      ...item,
      isAdded: false
    }))
    cartItems.value = []

    return data
  } catch (err) {
    console.log(err)
  }
}

watch(
  cartItems,
  () => {
    localStorage.setItem('cart', JSON.stringify(cartItems.value))
  },
  { deep: true }
)

provide('cartItems', cartItems)
provide('addToCart', addToCart)
provide('removeFromCart', removeFromCart)
</script>

<template>
  <Drawer
    v-if="isDrawerOpen"
    :total-price="totalPrice"
    @close-drawer="closeDrawer"
    @create-order="createOrder"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl my-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>
