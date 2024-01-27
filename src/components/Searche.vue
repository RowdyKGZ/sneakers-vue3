<script setup>
defineProps({
  onChangeSelect: Function,
  onChangeInput: Function
})
</script>

<template>
  <div class="flex gap-4">
    <select @change="onChangeSelect" class="py-2 px-3 border rounded-xl">
      <option value="name">By name</option>
      <option value="price">By price</option>
    </select>

    <div class="relative">
      <img src="/search.svg" alt="search" class="absolute top-3.5 left-3" />
      <input
        @input="onChangeInput"
        type="text"
        placeholder="Serach"
        class="border rounded-md py-2 pl-8 outline-none focus:border-gray-400"
      />
    </div>
  </div>
</template>
