<script setup>
import { onMounted, ref, watch, reactive } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
import Searche from './components/Searche.vue'

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = filters.searchQuery
    }

    const url = `https://611f67779771bf001785c902.mockapi.io/items`

    const { data } = await axios.get(url, { params })

    items.value = data

    console.log(items.value)
  } catch (err) {
    console.log(err)
  }
}

onMounted(fetchItems)

watch(filters, fetchItems)

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeInput = (event) => {
  filters.searchQuery = event.target.value
}
</script>

<template>
  <Drawer />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold">All sneakers</h2>

        <Searche :on-change-select="onChangeSelect" :on-change-input="onChangeInput" />
      </div>

      <CardList :items="items" />
    </div>
  </div>
</template>

<style scoped></style>
