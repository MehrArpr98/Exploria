<template>
  <div class="p-8">
    <div class="pb-4 mb-4 border-b border-gray-300">
      <div
        class="flex items-center border border-teal-500 rounded-full p-3 w-max"
      >
        <input
          v-model="filterText"
          class="appearance-none bg-transparent border-none w-80 text-gray-700 leading-tight outline-none focus:w-96 transition-all duration-700 ease-in-out"
          type="text"
          placeholder="Enter name, phone number or address..."
          aria-label="Filter text"
        />
      </div>
    </div>

    <app-table
      :records="processedRecords"
      :setSortType="setSortType"
      :sortType="sortType"
    />

    <pagination
      v-model="currentPage"
      :numberOfPages="Math.ceil(filteredRecords.length / perPage)"
      :currentPage="currentPage"
      @update:currentPage="
        (page) => {
          currentPage = page;
        }
      "
    />
  </div>
</template>

<script setup>
import records from "../assets/records.json";
import Pagination from "../components/pagination.vue";
import AppTable from "../components/table.vue";
import { computed, ref, onMounted } from "vue";
import { useRouter, useRoute } from "vue-router";

const currentPage = ref(1);
const perPage = 20;
const sortType = ref("");
const filterText = ref("");
const filteredRecords = ref([...records]);
const router = useRouter();
const route = useRoute();

const processedRecords = computed(() => {
  filteredRecords.value = records.filter(
    (record) =>
      record.name.toLowerCase().includes(filterText.value.toLowerCase()) ||
      record.phone.toLowerCase().indexOf(filterText.value.toLowerCase()) !==
        -1 ||
      record.address.toLowerCase().indexOf(filterText.value.toLowerCase()) !==
        -1
  );
  if (sortType.value === "asc") {
    sortAsc();
  } else if (sortType.value === "desc") {
    sortDesc();
  }

  router.push({ name: "home", query: makeQueryParams() });
  return filteredRecords.value.slice(
    (currentPage.value - 1) * perPage,
    currentPage.value * perPage
  );
});

onMounted(() => {
  const queryParams = route.query;

  if (queryParams.sortType && queryParams.sortType === "desc")
    sortType.value = "desc";
  else if (queryParams.sortType && queryParams.sortType === "asc")
    sortType.value = "asc";

  if (queryParams.page) currentPage.value = queryParams.page;
});
function setSortType() {
  if (sortType.value === "asc") {
    sortType.value = "desc";
  } else {
    sortType.value = "asc";
  }
}

function sortAsc() {
  filteredRecords.value.sort((a, b) => Date.parse(a.date) - Date.parse(b.date));
}

function sortDesc() {
  filteredRecords.value.sort((a, b) => Date.parse(b.date) - Date.parse(a.date));
}
function makeQueryParams() {
  const queryParams = {};
  if (sortType.value === "asc") queryParams.sortType = "asc";
  else if (sortType.value === "desc") queryParams.sortType = "desc";

  if (currentPage.value !== 1) queryParams.page = currentPage.value;

  return queryParams;
}
</script>
