<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";
import { useToast } from "vue-toastification";
import { useAuthStore } from "@/stores/AuthStore";

const claims = ref([]);
const toast = useToast();
const authStore = useAuthStore();

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
  <div class="max-w-xl mt-8 mx-auto p-6 bg-white rounded shadow">
    <h2 class="text-2xl font-bold mb-4">My Claims</h2>

    <div v-if="claims.length === 0" class="text-gray-500">
      You have no claims.
    </div>

    <div
      v-for="claim in claims"
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
