<script lang="ts" setup>
import { ref, defineProps, computed } from 'vue';

// Props to receive the selected tags for filtering
const props = defineProps<{ filterTags: string[] }>();

// Reactive array to store uploaded photos
const photos = ref<{ file: File; url: string; tags: string[] }[]>([]);

const handleFileInput = (event: Event) => {
  const target = event.target as HTMLInputElement;
  if (target.files) {
    const newPhotos = Array.from(target.files).map((file) => ({
      file,
      url: URL.createObjectURL(file),
      tags: [], // Initialize tags as an empty array
    }));
    photos.value = [...photos.value, ...newPhotos];
  }
};

const handleDelete = (index: number) => {
  photos.value.splice(index, 1);
};

const newTags = ref<{ [key: number]: string }>({});
const addTag = (photoIndex: number) => {
  const tag = newTags.value[photoIndex]?.trim();
  if (tag && !photos.value[photoIndex].tags.includes(tag)) {
    photos.value[photoIndex].tags.push(tag);
  }
  newTags.value[photoIndex] = '';
};

const filteredPhotos = computed(() => {
  if (props.filterTags.length === 0) {
    return photos.value;
  }
  return photos.value.filter(photo =>
    props.filterTags.some(tag =>
      photo.tags.some(photoTag => photoTag.toLowerCase() === tag.toLowerCase())
    )
  );
});
</script>

<template>
  <div class="min-h-screen p-5 font-mono bg-gray-100">
    <!-- File upload input -->
    <input type="file" multiple @change="handleFileInput" class="block w-full max-w-sm mx-auto mb-5 p-2 border border-gray-300 rounded" />

    <!-- Uploaded Photos Gallery -->
    <div v-if="filteredPhotos.length" class="photo-gallery container mx-auto">
      <h3 class="text-center text-xl font-bold mb-5">Uploaded Photos:</h3>
      <ul class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
        <li v-for="(photo, index) in filteredPhotos" :key="index" class="flex flex-col items-center bg-white p-4 rounded shadow-lg">
          <img :src="photo.url" :alt="photo.file.name" class="w-full h-auto mb-3 border border-gray-300 rounded" />
          <span class="text-sm font-bold mb-3">{{ photo.file.name }}</span>
          <button @click="handleDelete(index)" class="mt-2 px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600">
            Delete
          </button>
          <div class="tagging-component mt-5 w-full text-center">
            <h4 class="font-bold mb-3">Tags for this Photo:</h4>
            <div class="flex flex-wrap justify-center mb-2">
              <span v-for="tag in photo.tags" :key="tag" class="text-xs bg-blue-500 text-white rounded-full px-3 py-1 mr-2 mb-2">
                {{ tag }}
              </span>
            </div>
            <input type="text" v-model="newTags[index]" placeholder="Add a tag" class="block w-full p-2 border border-gray-300 rounded mb-2" />
            <button @click="addTag(index)" class="mt-1 px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600">
              Add Tag
            </button>
          </div>
        </li>
      </ul>
    </div>
    <div v-else class="text-center mt-10 text-gray-700">
      <p>No photos to display.</p>
    </div>
  </div>
</template>

<style scoped>
/* Add minimal styles here or leave styling for Tailwind later */
</style>
