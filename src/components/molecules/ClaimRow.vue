<script setup>
import { ref, watch } from "vue";
import { useToast } from "vue-toastification";

const props = defineProps({
  claim: Object,
});
const emit = defineEmits(["update-status"]);
const toast = useToast();

const localStatus = ref(props.claim.status);
const editing = ref(false);
const showHistory = ref(false);

watch(
  () => props.claim.status,
  (newStatus) => {
    localStatus.value = newStatus;
  }
);

const startEditing = () => (editing.value = true);
const cancelEditing = () => {
  localStatus.value = props.claim.status;
  editing.value = false;
};
const saveStatus = () => {
  if (localStatus.value === props.claim.status) {
    toast.info("Status not changed.");
    editing.value = false;
    return;
  }
  emit("update-status", props.claim.claimId, localStatus.value);
  editing.value = false;
};
</script>

<template>
  <tr class="border-b">
    <td class="px-4 py-2">{{ claim.foundItemId }}</td>
    <td class="px-4 py-2">{{ claim.description }}</td>
    <td class="px-4 py-2">
      <select
        v-model="localStatus"
        :disabled="!editing"
        class="border rounded px-2 py-1"
      >
        <option value="UNDER_REVIEW">UNDER_REVIEW</option>
        <option value="APPROVED">APPROVED</option>
        <option value="REJECTED">REJECTED</option>
        <option value="AWAITING_COLLECTION">AWAITING_COLLECTION</option>
        <option value="ITEM_COLLECTED">ITEM_COLLECTED</option>
      </select>
    </td>
    <td class="px-4 py-2">
      {{ new Date(claim.submittedAt).toLocaleString() }}
    </td>

    <td class="px-4 py-2">
      <div class="flex items-center space-x-2">
        <button
          v-if="!editing"
          @click="startEditing"
          class="bg-blue-500 text-white px-3 py-1 rounded"
        >
          Edit
        </button>

        <button
          v-if="editing"
          @click="saveStatus"
          class="bg-green-500 text-white px-3 py-1 rounded"
        >
          Save
        </button>
        <button
          v-if="editing"
          @click="cancelEditing"
          class="bg-gray-400 text-white px-3 py-1 rounded"
        >
          Cancel
        </button>

        <button
          @click="showHistory = !showHistory"
          class="text-blue-600 underline hover:text-blue-800 text-sm"
        >
          {{ showHistory ? "Hide History" : "Show History" }}
        </button>
      </div>
    </td>
  </tr>

  <tr v-if="showHistory" class="bg-gray-50">
    <td colspan="5" class="px-4 py-2">
      <ul class="space-y-1 text-sm text-gray-700">
        <li
          v-for="(entry, index) in claim.statusHistory"
          :key="index"
          class="flex justify-between"
        >
          <span>Status: {{ entry.oldStatus }} → {{ entry.newStatus }}</span>
          <span>{{ new Date(entry.changedAt).toLocaleString() }}</span>
        </li>
      </ul>
    </td>
  </tr>
</template>
