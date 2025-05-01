<template>
  <div class="flex items-start space-x-4">
    <span
      :class="[
        'inline-block text-sm font-medium px-3 py-1 rounded-full mt-1',
        bgColor,
        textColor,
      ]"
    >
      {{ label }}
    </span>
    <div>
      <h3 class="text-xl font-semibold text-gray-800">{{ title }}</h3>
      <p class="text-gray-600">{{ description }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "StatusItem",
  props: {
    label: String,
    title: String,
    description: String,
    bgColor: String,
    textColor: String,
  },
};
</script>
