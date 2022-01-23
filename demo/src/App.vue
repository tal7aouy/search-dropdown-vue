<script setup>
import { ref, reactive } from 'vue';
import SearchDropdown from 'search-dropdown-vue';
const countries = ref([
  { id: 1, name: 'Morocco' },
  { id: 2, name: 'USA' },
  { id: 3, name: 'France' },
  { id: 4, name: 'China' },
  { id: 5, name: 'Canada' },
]);
let object = reactive({ id: null, name: null });

const onSelectedOption = (payload) => (object = payload);
</script>

<template>
  <SearchDropdown
    :class="dropdown"
    :options="countries"
    v-on:selected="onSelectedOption"
  />
</template>
