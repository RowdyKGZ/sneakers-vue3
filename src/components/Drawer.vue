<script setup>
import DrawerHead from './DrawerHead.vue'
import CardListItem from './CartListItem.vue'
import InfoBlock from './InfoBlock.vue'

import { inject } from 'vue'

const emit = defineEmits(['createOrder'])

defineProps({
  totalPrice: Number,
  vatPrice: Number,
  disabledButton: Boolean
})

const toggleDrawer = inject('toggleDrawer')
</script>

<template>
  <div
    @click="toggleDrawer"
    class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"
  ></div>

  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 flex flex-col">
    <DrawerHead />

    <InfoBlock
      v-if="!totalPrice"
      title="Empty cart"
      description="Add the item to your cart to display"
      image-url="/package-icon.png"
    />

    <div v-else>
      <CardListItem />
      <div class="flex flex-col gap-3 my-4">
        <div class="flex gap-2">
          <span>Outcome</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }}$</b>
        </div>

        <div class="flex gap-2">
          <span>Tax 5%</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }}$</b>
        </div>

        <button
          :disabled="disabledButton"
          class="bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 transition cursor-pointer active:bg-lime-700 disabled:bg-slate-300 disabled:cursor-not-allowed"
          @click="() => emit('createOrder')"
        >
          Place an order
        </button>
      </div>
    </div>
  </div>
</template>
