<script setup>
import DrawerHead from './DrawerHead.vue'

import CardListItem from './CartListItem.vue'
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>

  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <CardListItem />

    <div class="flex flex-col gap-3 my-4">
      <div class="flex gap-2">
        <span>Outcome</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>500$</b>
      </div>

      <div class="flex gap-2">
        <span>Tax 5%</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>5$</b>
      </div>

      <button
        disabled
        class="bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 transition cursor-pointer active:bg-lime-700 disabled:bg-slate-300"
      >
        Place an order
      </button>
    </div>
  </div>
</template>
