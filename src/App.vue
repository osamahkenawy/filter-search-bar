<template>
  <div class="app-container">
    <FilterSearchBar @update:search="handleSearch" />
  </div>
</template>

<script>
import FilterSearchBar from './components/FilterSearchBar.vue';

export default {
  components: {
    FilterSearchBar,
  },
  methods: {
    handleSearch({ searchQuery, searchType }) {
      console.log(`Search Query: ${searchQuery}, Search Type: ${searchType}`);
    },
  },
};
</script>

<style>
/* Any custom styling */
.app-container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
}
</style>
