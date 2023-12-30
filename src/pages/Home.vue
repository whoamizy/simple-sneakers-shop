<script setup>
import { inject, onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'
import CardList from '../components/CardList.vue'

const cartItems = inject('cartItems')
const addToCart = inject('addToCart')
const removeFromCart = inject('removeFromCart')

const items = ref([])

const filters = reactive({
  sortBy: 'name',
  searchQuery: ''
})

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const newFavoriteItem = {
        item_id: item.id
      }

      item.isFavorite = true
      const { data } = await axios.post(
        'https://d585671a9582f90c.mokky.dev/favorites',
        newFavoriteItem
      )
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://d585671a9582f90c.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const onClickAddToCart = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://d585671a9582f90c.mokky.dev/items', { params })
    items.value = data.map((item) => ({
      ...item,
      isAdded: false,
      isFavorite: false,
      favoriteId: null
    }))
  } catch (e) {
    console.log(e)
  }
}

const fetchFavorites = async () => {
  try {
    const { data } = await axios.get('https://d585671a9582f90c.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = data.find((favorite) => favorite.item_id === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

watch(filters, async () => {
  await fetchItems()
})
onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cartItems.value = localCart ? JSON.parse(localCart) : []
  await fetchItems()
  await fetchFavorites()
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id)
  }))
})
</script>

<template>
  <div class="flex justify-between">
    <h2 class="text-3xl font-bold">Все кроссовки</h2>
    <div class="flex items-center gap-4">
      <select
        v-model="filters.sortBy"
        class="py-2 px-3 border border-gray-200 focus:border-gray-400 rounded-md focus:outline-none"
      >
        <option value="name">По названию</option>
        <option value="price">Сначала дешевые</option>
        <option value="-price">Сначала дорогие</option>
      </select>
      <div class="relative">
        <img class="absolute top-3 left-3" src="/search.svg" alt="Search" />
        <input
          v-model="filters.searchQuery"
          class="border rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400 transition"
          type="text"
          placeholder="Поиск..."
        />
      </div>
    </div>
  </div>
  <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddToCart" />
</template>
