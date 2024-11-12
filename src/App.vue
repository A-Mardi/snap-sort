<script lang="ts" setup>
import { ref } from 'vue';
import PhotoFilter from './components/PhotoFilter.vue';
import PhotoGallery from './components/PhotoGallery.vue';

const photos = ref<{ file: File; url: string; tags: string[] }[]>([]);
const filterTags = ref<string[]>([]);

const handleFileInput = (event: Event) => {
  const target = event.target as HTMLInputElement;
  if (target.files) {
    const newPhotos = Array.from(target.files).map((file) => ({
      file,
      url: URL.createObjectURL(file),
      tags: [],
    }));
    photos.value = [...photos.value, ...newPhotos];
  }
};

const updateFilterTags = (tags: string[]) => {
  filterTags.value = tags;
};
</script>

<template>
  <div class="min-h-screen bg-gray-100 text-gray-900 font-mono flex flex-col items-center p-8">
    <nav class="w-full max-w-5xl flex justify-between items-center mb-10">
      <div class="text-2xl font-bold text-blue-600">
        Snap Sort
      </div>
      <div class="space-x-4">
        <a href="#" class="text-sm font-medium text-gray-800 hover:text-blue-600 transition-colors">Home</a>
        <a href="#" class="text-sm font-medium text-gray-800 hover:text-blue-600 transition-colors">Gallery</a>
      </div>
    </nav>

    <div class="w-full max-w-5xl">
      <div class="bg-white p-6 rounded-lg shadow-md mb-10 border border-gray-200 max-w-2xl mx-auto">
        <h2 class="text-xl font-bold mb-4">Upload Your Photos</h2>
        <input type="file" multiple @change="handleFileInput" class="block w-full max-w-md mb-5 p-2 border border-gray-300 rounded" />
      </div>
      <div class="max-w-2xl mx-auto mb-10">
        <PhotoFilter @filter-change="updateFilterTags" />
      </div>
      <div v-if="photos.length" class="max-w-5xl mx-auto mb-10">
        <PhotoGallery :photos="photos" :filter-tags="filterTags" />
      </div>
      <div v-else class="text-center mt-10 text-gray-700">
        <p>No photos to display.</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
