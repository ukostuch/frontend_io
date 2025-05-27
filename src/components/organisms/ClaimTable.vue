<script setup>
import { ref, computed, onMounted } from "vue";
import axios from "axios";
import { useToast } from "vue-toastification";
import { useAuthStore } from "@/stores/AuthStore";
import ClaimRow from "@/components/molecules/ClaimRow.vue";

const claims = ref([]);
const searchItemId = ref("");
const toast = useToast();
const authStore = useAuthStore();

const fetchClaims = async () => {
  try {
    const response = await axios.get("/api/claims/admin-view", {
      headers: {
        Authorization: `Bearer ${authStore.getToken}`,
      },
    });
    claims.value = response.data;
  } catch (error) {
    toast.error("Failed to fetch claims.");
  }
};

const updateClaimStatus = async (claimId, newStatus) => {
  try {
    await axios.put(`/api/claims/${claimId}/status`, null, {
      params: { newStatus },
      headers: {
        Authorization: `Bearer ${authStore.getToken}`,
      },
    });
    toast.success("Status updated.");
    await fetchClaims();
  } catch (error) {
    toast.error("Failed to update status.");
  }
};

onMounted(fetchClaims);

const filteredClaims = computed(() =>
  claims.value.filter((claim) =>
    String(claim.foundItemId)
      .toLowerCase()
      .includes(searchItemId.value.toLowerCase())
  )
);
</script>

<template>
  <div class="max-w-7xl mx-auto p-6 bg-white rounded shadow mt-4">
    <h2 class="text-2xl font-bold mb-4">Admin - All Claims</h2>
    <h5>Filter by Item ID:</h5>
    <input
      v-model="searchItemId"
      type="text"
      placeholder="Paste Item ID here..."
      class="border px-3 py-1 rounded mb-4 w-full max-w-xs"
    />

    <div v-if="filteredClaims.length === 0" class="text-gray-500">
      No claims available.
    </div>

    <table v-else class="min-w-full table-auto">
      <thead>
        <tr class="bg-gray-100">
          <th class="px-4 py-2 text-left">Item ID</th>
          <th class="px-4 py-2 text-left">Description</th>
          <th class="px-4 py-2 text-left">Status</th>
          <th class="px-4 py-2 text-left">Submitted At</th>
          <th class="px-4 py-2 text-left">Actions</th>
        </tr>
      </thead>
      <tbody>
        <ClaimRow
          v-for="claim in filteredClaims"
          :key="claim.claimId"
          :claim="claim"
          @update-status="updateClaimStatus"
        />
      </tbody>
    </table>
  </div>
</template>
