<script setup>
defineProps({
  title: String,
  imageUrl: String,
  price: String,
  isFavorite: Boolean,
  isAdded: Boolean,
  onCLickAdd: Function,
  onCLickFaorite: Function
})
</script>

<template>
  <div
    class="relative bg-white m-10 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
  >
    <img
      @click="onCLickFaorite"
      :src="isFavorite ? `/like-2.svg` : `/like-1.svg`"
      alt="Like 1"
      class="absolute top-8 left-8"
    />

    <img :src="imageUrl" alt="Sneaker" />
    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-500">Price</span>
        <b>{{ price }}$</b>
      </div>

      <img @click="onCLickAdd" :src="isAdded ? `/checked.svg` : `/plus.svg`" alt="add sneakers" />
    </div>
  </div>
</template>
