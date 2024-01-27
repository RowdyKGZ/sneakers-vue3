<script setup>
// import { inject } from 'vue'

import Card from './Card.vue'

defineProps({
  items: Array
})

// const addToFavorite = inject('addToFavorite')

const emit = defineEmits(['addToFavorite'])
</script>

<template>
  <div class="grid grid-cols-3">
    <Card
      v-for="item in items"
      :id="item.id"
      v-bind:key="item.id"
      :image-url="item.img"
      :title="item.title"
      :price="item.price"
      :is-favorite="item.isFavorite"
      :onClickFavorite="() => emit('addToFavorite', item)"
    />
  </div>
</template>
