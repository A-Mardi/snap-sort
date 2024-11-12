<script lang="ts" setup>
import { ref } from 'vue';
import PhotoFilter from './PhotoFilter.vue';
import PhotoGallery from './PhotoGallery.vue';

// Reactive state to store photos and filter tags
const photos = ref<{ file: File; url: string; tags: string[] }[]>([]);
const filterTags = ref<string[]>([]);

// Function to handle file input
const handleFileInput = (event: Event) => {
  const target = event.target as HTMLInputElement;
  if (target.files) {
    const newPhotos = Array.from(target.files).map((file) => ({
      file,
      url: URL.createObjectURL(file),
      tags: [], // Initialize with an empty array of tags
    }));
    photos.value = [...photos.value, ...newPhotos];
  }
};

// Function to handle updating filter tags from PhotoFilter
const updateFilterTags = (tags: string[]) => {
  filterTags.value = tags;
};
</script>

<template>
  <div class="min-h-screen p-8 font-mono">
    <!-- Upload Section -->
    <div class="bg-white p-6 rounded-lg shadow-md mb-10 border border-gray-200 max-w-2xl mx-auto">
      <h2 class="text-xl font-bold mb-4">Upload Your Photos</h2>
      <input type="file" multiple @change="handleFileInput" class="block w-full max-w-md mb-5 p-2 border border-gray-300 rounded" />
    </div>

    <!-- Filter Component -->
    <div class="max-w-2xl mx-auto mb-10">
      <PhotoFilter @filter-change="updateFilterTags" />
    </div>

    <!-- Photo Gallery Component with Filtering -->
    <div v-if="photos.length" class="max-w-5xl mx-auto mb-10">
      <PhotoGallery :photos="photos" :filter-tags="filterTags" />
    </div>
  </div>
</template>

<style scoped>
</style>
