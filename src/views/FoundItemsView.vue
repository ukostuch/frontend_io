<script setup>
import ItemCards from '@/components/organisms/ItemCards.vue';
</script>

<template>
  <div class="w-full bg-my-white py-10 flex flex-col items-center">
    <div class="w-full">
      <div class="login-caption font-bold text-my-green text-2xl text-center mb-4">
        Found items
      </div>
      <div class="login-caption font-bold text-my-green text-base text-center mb-4">
        Find your lost item using avaliable filters
      </div>
      <ItemCards />
    </div>
  </div>
</template>
