<script setup>
import { computed, ref, nextTick } from 'vue';
import HeaderSearch from './header-search.vue';

const props = defineProps({
  value: {
    type: String,
    required: true,
  },
  searchTypesProp: {
    type: Array,
    default: () => ['All', 'Plate №', 'Categories', 'Users']
  },
});

const emit = defineEmits(['update:search']);

const searchFieldRef = ref(null);
const isSearchActive = ref(false);
const selectedSearchType = ref('All');
const searchQuery = ref(props.value);

const searchTypes = computed(() => props.searchTypesProp);

const enableSearch = async () => {
  isSearchActive.value = true;
  await nextTick();
  searchFieldRef.value?.focus();
};

// Handle search action and close if appropriate
const handleSearch = async () => {
  if (searchQuery.value) {
    emit('update:search', { searchQuery: searchQuery.value, searchType: selectedSearchType.value });
  }
  // Close search bar regardless of query status
  isSearchActive.value = false;
};

// Update searchQuery as user types
const updateSearchQuery = (event) => {
  searchQuery.value = event.target.value;
};
</script>

<template>
  <div class="search-bar-container d-flex align-items-center">
    <div v-if="isSearchActive" class="input-group">
      <!-- Search Type Dropdown -->
      <select v-model="selectedSearchType" class="form-select search-type-dropdown">
        <option v-for="(type, index) in searchTypes" :key="index">{{ type }}</option>
      </select>

      <!-- Main Search Input -->
      <input
        type="text"
        class="form-control search-input"
        placeholder="Search"
        ref="searchFieldRef"
        v-model="searchQuery"
        @input="updateSearchQuery"
      />

      <!-- Append Icon Button triggers handleSearch -->
      <span class="input-group-text append-icon" @click="handleSearch">
        <HeaderSearch strokeColor="#FFF" />
      </span>
    </div>

    <slot v-else name="activator" :on="{ click: enableSearch }">
      <span style="border: 2px solid transparent">
        <v-btn
          @click="enableSearch"
          :elevation="1"
          class="white"
          icon
        >
        <HeaderSearch strokeColor="#1a73eb" />
        </v-btn>
      </span>
    </slot>
  </div>
</template>

<style scoped>
.search-bar-container {
  width: 100%;
  max-width: 600px;
}

/* Main input group styling */
.input-group {
  display: flex;
  align-items: center;
  width: 100%;
  border: 1px solid #ccc;
  border-radius: 12px;
  overflow: hidden;
}

/* Search type dropdown styling */
.search-type-dropdown {
  border: none;
  padding: 8px;
  font-size: 0.9rem;
  background-color: #f8f9fa;
  border-top-left-radius: 12px;
  border-bottom-left-radius: 12px;
  color: #1a73eb;
  text-align: center;
  min-width: 120px;
  font-weight: bold;
}

/* Main search input styling */
.search-input {
  flex-grow: 1;
  border: none;
  padding: 8px;
  font-size: 0.9rem;
  border-left: 1px solid #ccc;
}

/* Append icon styling */
.append-icon {
  background-color: #1a73eb;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px;
  cursor: pointer;
  border-top-right-radius: 12px;
  border-bottom-right-radius: 12px;
}
</style>
