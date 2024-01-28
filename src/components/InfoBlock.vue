<script setup>
defineProps({ title: String, imageUrl: String, description: String })
</script>

<template>
  <div class="my-auto flex flex-col items-center text-center max-w-[285px] mx-auto">
    <img :src="imageUrl" alt="image" class="" width="80" height="80" />

    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>

    <p class="text-gray-400">{{ description }}</p>
  </div>
</template>
