<script lang="ts" setup>
import { ref } from 'vue';
import PhotoGallery from './components/PhotoGallery.vue';
import PhotoFilter from './components/PhotoFilter.vue';

const selectedTags = ref<string[]>([]);

// Handle filter change from PhotoFilter component
const handleFilterChange = (tags: string[]) => {
  selectedTags.value = tags;
};
</script>

<template>
  <div id="app">
    <h1>Welcome to Snap Sort</h1>

    <!-- Filtering Component to select tags to filter images -->
    <PhotoFilter @filter-change="handleFilterChange" />

    <!-- Combined upload and gallery component -->
    <PhotoGallery :filter-tags="selectedTags" />
  </div>
</template>

<style scoped>
/* Add any necessary global styling here */
</style>
