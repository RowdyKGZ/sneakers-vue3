<script setup>
import Card from './Card.vue'

const onClickAdd = () => {
  alert('Hello')
}
</script>

<template>
  <div class="grid grid-cols-3">
    <Card
      image-url="/sneakers/sneakers-1.jpg"
      title="Men's Nike Sneakers"
      :price="100"
      is-favorite
      :onCLickAdd="onClickAdd"
    />
    <Card image-url="/sneakers/sneakers-2.jpg" title="Men's Nike Sneakers" :price="350" />
    <Card image-url="/sneakers/sneakers-3.jpg" title="Men's Nike Sneakers" :price="400" is-added />
  </div>
</template>
