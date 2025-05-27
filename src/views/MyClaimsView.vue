<script setup>
import { onMounted, ref, computed } from "vue";
import axios from "axios";
import { useToast } from "vue-toastification";
import { useAuthStore } from "@/stores/AuthStore";

const claims = ref([]);
const toast = useToast();
const authStore = useAuthStore();
const sortOrder = ref("desc"); // "asc" or "desc"

const sortedClaims = computed(() => {
  return [...claims.value].sort((a, b) => {
    const dateA = new Date(a.submittedAt);
    const dateB = new Date(b.submittedAt);
    return sortOrder.value === "asc" ? dateA - dateB : dateB - dateA;
  });
});

const fetchClaims = async () => {
  try {
    const token = authStore.getToken;
    console.log("Token sent to the server: ", token);

    const response = await axios.get("/api/claims/my", {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    });

    claims.value = response.data;
    console.log("Fetched claims: ", response.data);
  } catch (error) {
    if (error.response) {
      toast.error(
        `Failed to fetch claims: ${
          error.response.data.message || error.response.data
        }`
      );
    } else if (error.request) {
      console.error("Error request: ", error.request);
      toast.error("No response received from server.");
    } else {
      console.error("Error message: ", error.message);
      toast.error("An unexpected error occurred.");
    }
  }
};

onMounted(fetchClaims);
</script>

<template>
  <div
    class="login-caption font-bold text-my-green text-2xl text-center mb-4 mt-9"
  >
    My Claims
  </div>

  <div class="login-caption font-bold text-my-green text-base text-center mb-4">
    You can see all your claims here.
  </div>
  <div class="mb-6 mt-2 flex flex-col justify-center items-center">
    <label for="sortOrder" class="block mb-3 text-sm font-medium text-gray-700"
      >Sort by date:</label
    ><select
      id="sortOrder"
      v-model="sortOrder"
      class="bg-my-white text-my-gray p-2 border border-my-gray rounded"
    >
      <option value="desc">Newest first</option>
      <option value="asc">Oldest first</option>
    </select>
  </div>
  <div class="max-w-xl mt-8 mx-auto p-6 bg-white rounded shadow">
    <div v-if="claims.length === 0" class="text-gray-500">
      You have no claims.
    </div>

    <div
      v-for="claim in sortedClaims"
      :key="claim.id"
      class="border p-4 mb-4 rounded"
    >
      <p><strong>Description:</strong> {{ claim.description }}</p>
      <p>
        <strong>Status:</strong>
        <span
          :class="{
            'text-gray-600': claim.status === 'UNDER_REVIEW',
            'text-green-600': claim.status === 'APPROVED',
            'text-red-600': claim.status === 'REJECTED',
            'text-yellow-600': claim.status === 'AWAITING_COLLECTION',
            'text-blue-600': claim.status === 'ITEM_COLLECTED',
          }"
        >
          {{ claim.status }}
        </span>
      </p>
      <p>
        <strong>Submitted:</strong>
        {{ new Date(claim.submittedAt).toLocaleString() }}
      </p>
    </div>
  </div>
</template>
