<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'

import CardList from '@/components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://611f67779771bf001785c902.mockapi.io/favorites')

    favorites.value = data.map((obj) => obj.item)
  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
  <CardList :items="favorites" />
</template>
