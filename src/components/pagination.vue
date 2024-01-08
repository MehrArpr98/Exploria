<template>
  <div class="pagination flex flex-nowrap py-6">
    <ul
      v-if="numberOfPages >= 1"
      class="bg-teal-500 bg-white m-0 p-4 flex border-0 rounded-2xl items-center overflow-auto content-center"
    >
      <li
        class="flex cursor-pointer select-none"
        aria-label="go to previous page"
        @click="previous()"
        :class="{
          'cursor-not-allowed [&>div]:hover:bg-white': currentPage == 1,
        }"
      >
        <div
          class="text-gray-500 rounded-md w-8 h-8 py-1.5 text-center text-sm font-bold hover:border-0 hover:text-gray-800 hover:bg-teal-300"
        >
          &laquo;
        </div>
      </li>
      <li
        v-for="index in numberOfPages"
        :key="index"
        :aria-label="'go to page ' + index"
        class="flex cursor-pointer select-none"
        @click="setCurrentPage(index)"
      >
        <div
          class="text-gray-500 rounded-md w-8 h-8 mx-1.5 py-1.5 text-center text-sm font-bold hover:border-0 hover:text-gray-800 hover:bg-teal-300"
          :class="{
            'bg-teal-500 text-white hover:border-0': currentPage == index,
          }"
        >
          {{ index }}
        </div>
      </li>
      <li
        class="flex cursor-pointer select-none"
        :class="{
          'cursor-not-allowed [&>div]:hover:bg-white':
            currentPage == numberOfPages || !numberOfPages,
        }"
        aria-label="go to next page"
        @click="next()"
      >
        <div
          class="text-gray-500 rounded-md w-8 h-8 py-1.5 text-center text-sm font-bold hover:border-0 hover:text-gray-800 hover:bg-teal-300"
        >
          &raquo;
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { toRefs } from "vue";

const props = defineProps({
  numberOfPages: Number,
  currentPage: Number,
});

const { numberOfPages, currentPage } = toRefs(props);

const emit = defineEmits(["update:currentPage"]);

const setCurrentPage = (number) => {
  emit("update:currentPage", number);
};

const previous = () => {
  if (currentPage.value === 1) return;
  emit("update:currentPage", currentPage.value - 1);
};

const next = () => {
  if (currentPage.value >= numberOfPages.value) return;
  emit("update:currentPage", currentPage.value + 1);
};
</script>
