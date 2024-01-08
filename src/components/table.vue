<template>
  <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
    <table
      class="w-full text-sm text-left rtl:text-right text-gray-500"
    >
      <thead
        class="text-xs text-gray-700 uppercase bg-gray-50"
      >
        <tr>
          <th scope="col" class="px-6 py-3">Id</th>
          <th scope="col" class="px-6 py-3">Name</th>
          <th scope="col" class="px-6 py-3">
            Date
            <button @click="setSortType">
              <span v-if="sortType === 'asc'">▲</span> <span v-else>▼</span>
            </button>
          </th>
          <th scope="col" class="px-6 py-3">Address</th>
          <th scope="col" class="px-6 py-3">Phone</th>
        </tr>
      </thead>
      <tbody>
        <tr
          class="odd:bg-white even:bg-gray-50 border-b hover:bg-gray-200"
          v-for="(record, index) in props.records"
          :key="index"
        >
          <td class="px-6 py-4">{{ record.id }}</td>
          <td class="px-6 py-4">{{ record.name }}</td>
          <td class="px-6 py-4">{{ record.date }}</td>
          <td class="px-6 py-4">{{ record.address }}</td>
          <td class="px-6 py-4">{{ record.phone }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script setup>
const props = defineProps({
  records: Array,
  setSortType: Function,
  sortType: String
});
</script>
