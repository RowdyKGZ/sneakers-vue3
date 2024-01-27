<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})
</script>

<template>
  <div class="grid grid-cols-3">
    <Card
      v-for="item in items"
      v-bind:key="item.id"
      :image-url="item.img"
      :title="item.title"
      :price="item.price"
      :is-favorite="item.isFavorite"
    />
  </div>
</template>
