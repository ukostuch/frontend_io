<template>
  <section class="mb-8">
    <h2 class="text-2xl font-semibold text-my-green mb-4">Claim statuses:</h2>
    <div class="space-y-6">
      <StatusItem
        label="Under review"
        title="Under review"
        description="Once you click the 'Claim ownership' button, you must immediately provide a detailed description of the item. You may also upload a photo. For example, if it's a backpack, describe its contents. After submitting, the status becomes 'Under review' by administrator."
        bgColor="bg-gray-200"
        textColor="text-gray-800"
      />
      <StatusItem
        label="Approved"
        title="Approved"
        description="If the description matches the item, the administrator will approve the claim. You can then visit the designated office to collect your item."
        bgColor="bg-green-100"
        textColor="text-green-800"
      />
      <StatusItem
        label="Rejected"
        title="Rejected"
        description="If the description is too vague or inconsistent with the item, the administrator will reject your claim."
        bgColor="bg-red-100"
        textColor="text-red-800"
      />
      <StatusItem
        label="Awaiting collection"
        title="Awaiting collection"
        description="If your claim has been accepted, the status will be updated to 'Awaiting collection' until you retrieve the item from the office."
        bgColor="bg-orange-100"
        textColor="text-orange-800"
      />
      <StatusItem
        label="Item collected"
        title="Item collected"
        description="After collecting the item, the status will change to: 'Item collected'."
        bgColor="bg-blue-100"
        textColor="text-blue-800"
      />
    </div>
  </section>
</template>

<script>
import StatusItem from "@/components/molecules/StatusItem.vue";

export default {
  name: "StatusList",
  components: {
    StatusItem,
  },
};
</script>
