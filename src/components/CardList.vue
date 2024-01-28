<script setup>
// import { inject } from 'vue'

import Card from './Card.vue'

defineProps({
  items: Array,
  addToCart: Function
})

// const addToFavorite = inject('addToFavorite')

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
  <div class="grid grid-cols-3" v-auto-animate>
    <Card
      v-for="item in items"
      :id="item.id"
      v-bind:key="item.id"
      :image-url="item.img"
      :title="item.title"
      :price="item.price"
      :is-favorite="item.isFavorite"
      :is-added="item.isAdded"
      :onClickFavorite="() => emit('addToFavorite', item)"
      :onCLickAdd="() => emit('addToCart', item)"
    />
  </div>
</template>
