<script setup>
import OfficeCard from "@/components/molecules/OfficeCard.vue";
import OfficeMap from "@/components/organisms/OfficeMap.vue";
import { ref, onMounted, computed } from "vue";
import axios from "axios";

const offices = ref([]);
const selectedDistrict = ref("");

const districts = computed(() => {
  const allDistricts = offices.value.map((office) => office.district.name);
  const uniqueDistricts = [...new Set(allDistricts)];

  return uniqueDistricts.sort((a, b) => Number(a) - Number(b));
});

const filteredOffices = computed(() => {
  if (!selectedDistrict.value) {
    return offices.value;
  }
  return offices.value.filter(
    (office) => office.district.name === selectedDistrict.value
  );
});

onMounted(async () => {
  fetchData();
});

const fetchData = async () => {
  try {
    const response = await axios.get("/api/offices");
    offices.value = response.data;
  } catch (error) {
    console.error("Error fetching jobs", error);
  }
};
</script>
<template>
  <div class="mb-6 mt-2 flex flex-col justify-center items-center">
    <label for="district" class="block mb-3 text-sm font-medium text-gray-700"
      >Filter by District:</label
    >
    <select
      v-model="selectedDistrict"
      id="district"
      class="bg-my-white text-my-gray p-2 border border-my-gray rounded"
    >
      <option value="">All Districts</option>
      <option v-for="district in districts" :key="district" :value="district">
        {{ district }}
      </option>
    </select>
  </div>
  <section class="bg-my-white px-8 py-10">
    <div class="m-auto">
      <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-5 gap-6">
        <OfficeCard
          v-for="office in filteredOffices"
          :key="office.id"
          :details="office"
          @changed="fetchData"
        />
      </div>
    </div>
  </section>
  <div class="m-auto lg:px-8 sm:px-8 mt-10 w-full relative z-0">
    <OfficeMap
      v-if="filteredOffices.length > 0"
      :key="selectedDistrict"
      :offices="filteredOffices"
      v-bind="$attrs"
    />
  </div>
</template>
