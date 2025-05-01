<script setup>
import OfficeCards from "@/components/organisms/OfficeCards.vue";
import { RouterLink } from "vue-router";
import { useAuthStore } from "../stores/AuthStore";
import OfficeMap from "@/components/organisms/OfficeMap.vue";

const authStore = useAuthStore();
</script>

<template>
  <div
    v-if="authStore.getRole === 'ADMIN'"
    class="bg-my-white space-x-5 flex py-5 px-5 justify-end"
  >
    <RouterLink to="/offices/add">
      <button
        class="bg-my-green text-my-white font-bold text-sm px-3 py-2 rounded hover:bg-gray-900 hover:text-my-white transition duration-300"
      >
        Add office
      </button>
    </RouterLink>
  </div>
  <div class="w-full bg-my-white py-10 flex flex-col items-center">
    <div class="w-full">
      <div
        class="login-caption font-bold text-my-green text-2xl text-center mb-4"
      >
        Our offices
      </div>
      <div
        class="login-caption font-bold text-my-green text-base text-center mb-4"
      >
        Check our locations all around Krakow
      </div>
      <OfficeCards />
      <div class="lg:px-72 sm:px-24 mt-10 w-full relative z-0">
        <OfficeMap />
      </div>
    </div>
  </div>
</template>
