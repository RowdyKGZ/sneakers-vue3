<script setup>
import { ref, watch, provide, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

// Cart
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

const removeFromCart = (item) => {
  carts.value.splice(carts.value.indexOf(item), 1)
  item.isAdded = false
}
// Cart

const toggleDrawer = () => {
  drawerOpen.value = !drawerOpen.value
}

watch(
  carts,
  () => {
    localStorage.setItem('cart', JSON.stringify(carts.value))
  },
  { deep: true }
)
// deep

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
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>
