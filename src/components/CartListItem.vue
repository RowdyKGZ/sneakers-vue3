<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { carts, removeFromCart } = inject('carts')
</script>

<template>
  <div class="flex flex-col gap-4">
    <CartItem
      v-for="item in carts"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :img="item.img"
      :price="item.price"
      @removeFromCart="() => removeFromCart(item)"
    />
  </div>
</template>
