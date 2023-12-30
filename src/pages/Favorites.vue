<script setup>
import InfoBlock from '../components/InfoBlock.vue'
import CardList from '../components/CardList.vue'
import axios from 'axios'
import { onMounted, ref } from 'vue'

const favorites = ref([])

const fetchFavorites = async () => {
  try {
    const { data } = await axios.get(
      'https://d585671a9582f90c.mokky.dev/favorites?_relations=items'
    )
    favorites.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  }
}

onMounted(fetchFavorites)
</script>

<template>
  <h2 class="text-3xl font-bold">Мои закладки</h2>
  <InfoBlock
    v-if="!favorites.length"
    title="Закладок нет :("
    description="Вы ничего не добавляли в закладки"
    imageUrl="/emoji-1.png"
  />
  <CardList
    :items="favorites"
    @add-to-favorite="addToFavorite"
    @add-to-cart="onClickAddToCart"
    is-favorites
  />
</template>
