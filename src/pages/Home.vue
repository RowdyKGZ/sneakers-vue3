<script setup>
import CardList from '../components/CardList.vue'
import Searche from '../components/Searche.vue'

import axios from 'axios'
import { inject, reactive, watch, ref, onMounted, provide } from 'vue'

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const items = ref([])

const { carts, removeFromCart } = inject('carts')

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      item.isFavorite = true

      const obj = {
        item_id: item.id,
        item
      }

      const { data } = await axios.post(
        'https://611f67779771bf001785c902.mockapi.io/favorites',
        obj
      )

      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://611f67779771bf001785c902.mockapi.io/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (error) {
    console.log(error)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://611f67779771bf001785c902.mockapi.io/favorites`
    )

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id == item.id)

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

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = filters.searchQuery
    }

    const { data } = await axios.get(`https://611f67779771bf001785c902.mockapi.io/items`, {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false,
      favoriteId: null
    }))
  } catch (err) {
    console.log(err)
  }
}

const addToCart = (item) => {
  carts.value.push(item)
  item.isAdded = true
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeInput = (event) => {
  filters.searchQuery = event.target.value
}

onMounted(async () => {
  carts.value = JSON.parse(localStorage.getItem('cart') || '[]')

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: carts.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(carts, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(filters, fetchItems)
provide('addToFavorite', { addToFavorite, onClickAddPlus })
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold">All sneakers</h2>

    <Searche :on-change-select="onChangeSelect" :on-change-input="onChangeInput" />
  </div>

  <CardList :items="items" @addToFavorite="addToFavorite" @addToCart="onClickAddPlus" />
</template>
