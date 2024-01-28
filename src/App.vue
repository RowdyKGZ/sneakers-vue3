<script setup>
import { onMounted, ref, watch, reactive, provide, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
import Searche from './components/Searche.vue'

const items = ref([])
const carts = ref([])
const isCreatingOrder = ref(false)
const drawerOpen = ref(false)

const totalPrice = computed(() => carts.value.reduce((acc, item) => acc + +item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const cartIsEmpty = computed(() => carts.value.length === 0)
const cartDisabledButton = computed(() => isCreatingOrder.value || cartIsEmpty.value)

const createOrder = async () => {
  isCreatingOrder.value = true
  try {
    const { data } = await axios.post(`https://611f67779771bf001785c902.mockapi.io/orders`, {
      items: carts.value,
      totalPrice: totalPrice.value
    })

    carts.value = []

    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreatingOrder.value = false
  }
}

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://611f67779771bf001785c902.mockapi.io/favorites`
    )

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId == item.id)

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

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      item.isFavorite = true

      const obj = {
        parentId: item.id
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

const addToCart = (item) => {
  carts.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  carts.value.splice(carts.value.indexOf(item), 1)
  item.isAdded = false
}

const onClickAddPlus = async (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

watch(filters, fetchItems)

watch(carts, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
// deep

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeInput = (event) => {
  filters.searchQuery = event.target.value
}

const toggleDrawer = () => {
  drawerOpen.value = !drawerOpen.value
}

provide('addToFavorite', addToFavorite)
provide('toggleDrawer', toggleDrawer)
provide('carts', { carts, removeFromCart })
</script>

<template>
  <Drawer
    v-if="drawerOpen"
    :totalPrice="totalPrice"
    :vatPrice="vatPrice"
    @createOrder="createOrder"
    :disabledButton="cartDisabledButton"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header @toggleDrawer="toggleDrawer" :totalPrice="totalPrice" />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold">All sneakers</h2>

        <Searche :on-change-select="onChangeSelect" :on-change-input="onChangeInput" />
      </div>

      <CardList :items="items" @addToFavorite="addToFavorite" @addToCart="onClickAddPlus" />
    </div>
  </div>
</template>

<style scoped></style>
