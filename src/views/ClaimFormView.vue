<script setup>
import { ref } from "vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
import { useToast } from "vue-toastification";
import { useAuthStore } from "@/stores/AuthStore";

const route = useRoute();
const authStore = useAuthStore();
const router = useRouter();
const toast = useToast();

const foundItemId = route.params.id;
const description = ref("");
const loading = ref(false);

const submitClaim = async () => {
  if (!description.value.trim()) {
    toast.warning("Please provide a description.");
    return;
  }

  try {
    loading.value = true;
    const formData = new FormData();
    formData.append("foundItemId", foundItemId);
    formData.append("description", description.value);
    formData.append("attachment", new Blob([]), "empty.txt");

    await axios.post("/api/claims", formData, {
      headers: {
        Authorization: `Bearer ${authStore.getToken}`,
        "Content-Type": "multipart/form-data",
      },
    });

    toast.success("Claim submitted successfully.");
    router.push("/my-claims");
  } catch (error) {
    toast.error("Failed to submit claim.");
    console.error(error);
  } finally {
    loading.value = false;
  }
};
</script>

<template>
  <div class="max-w-xl mt-8 mx-auto p-6 bg-white rounded shadow">
    <h2 class="text-xl font-semibold mb-4">Submit Claim</h2>
    <label for="description" class="block mb-2 font-medium"
      >Why do you think this is your item?</label
    >
    <textarea
      id="description"
      v-model="description"
      class="w-full p-2 border border-gray-300 rounded"
      rows="5"
      placeholder="Describe proof of ownership..."
    ></textarea>

    <button
      @click="submitClaim"
      :disabled="loading"
      class="mt-4 bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700"
    >
      {{ loading ? "Submitting..." : "Submit Claim" }}
    </button>
  </div>
</template>
