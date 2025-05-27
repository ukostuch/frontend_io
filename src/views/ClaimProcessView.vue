<template>
  <div class="max-w-4xl mx-auto px-6 sm:px-8 py-10">
    <div class="bg-white shadow-md rounded-xl p-6 sm:p-10">
      <h1 class="text-4xl font-bold text-center mb-8">
        <span class="inline-block border-b-8 border-my-green pb-2">
          How to report ownership and claim a found item?
        </span>
      </h1>

      <section class="mb-10">
        <h2 class="text-2xl font-semibold text-my-green mb-4">
          The reporting and claim process
        </h2>
        <p class="text-gray-700 leading-relaxed">
          Reports of lost items found in Kraków can be found in the
          <a
            href="/found"
            class="text-blue-600 hover:text-blue-800 underline transition-colors duration-200"
          >
            FoundItems
          </a>
          tab. Most items are published without photos, with a brief
          description. If you suspect that it is your lost item and want to
          collect it from the
          <a
            href="/offices"
            class="text-blue-600 hover:text-blue-800 underline transition-colors duration-200"
          >
            Lost and found office </a
          >, you need to prove that it is your property. In this short guide, we
          will walk you through this process step by step.
        </p>
        <div class="mt-6 text-center">
          <router-link
            to="/my-claims"
            class="bg-my-green text-my-white font-bold text-sm px-3 py-2 rounded hover:bg-gray-900 hover:text-my-white transition duration-300"
          >
            See your claims
          </router-link>
        </div>
      </section>

      <StatusList />
    </div>
  </div>
</template>

<script>
import StatusList from "@/components/organisms/StatusList.vue";

export default {
  name: "ClaimProcess",
  components: {
    StatusList,
  },
};
</script>
