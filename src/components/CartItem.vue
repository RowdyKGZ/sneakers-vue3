<script setup>
const emit = defineEmits(['removeFromCart'])

defineProps({
  id: String,
  title: String,
  img: String,
  price: String
})
</script>

<template>
  <div class="flex items-center border border-slate-100 p-4 rounded-xl gap-4">
    <img :src="img" alt="sneaker" class="w-16 h-16" />

    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }}$</b>
        <img
          @click="emit('removeFromCart')"
          src="/close.svg"
          alt="closed"
          class="opacity-50 hover:opacity-100 cursor-pointer transition"
        />
      </div>
    </div>
  </div>
</template>
