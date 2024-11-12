<script lang="ts" setup>
import { ref, defineEmits } from 'vue';

const availableTags = ref<string[]>(['Info3170', 'Info5052', 'Info3140', 'Info3135', 'Info1219', 'Writ1043']);
const selectedTags = ref<string[]>([]);
const emit = defineEmits<{ (e: 'filter-change', tags: string[]): void }>();

const handleTagSelection = (tag: string) => {
  const normalizedTag = tag.toLowerCase(); // For case insensitive input
  const selectedIndex = selectedTags.value.findIndex(t => t.toLowerCase() === normalizedTag);

  if (selectedIndex !== -1) {
    selectedTags.value.splice(selectedIndex, 1);
  } else {
    selectedTags.value.push(tag);
  }
  emit('filter-change', selectedTags.value.map(t => t.toLowerCase()));
};

const newTagInput = ref('');
const addNewTag = () => {
  const newTag = newTagInput.value.trim().toLowerCase();
  if (newTag && !availableTags.value.some(tag => tag.toLowerCase() === newTag)) {
    availableTags.value.push(newTag);
    newTagInput.value = '';
  }
};
</script>

<template>
  <div class="filter-component bg-white p-6 rounded-lg shadow-md border border-gray-200 max-w-2xl mx-auto mt-10">
    <h3 class="text-lg font-bold text-gray-900 mb-4">Filter by Tags:</h3>
    <div class="flex flex-wrap mb-6">
      <label v-for="tag in availableTags" :key="tag" class="mr-4 mb-2 flex items-center cursor-pointer">
        <input type="checkbox" :value="tag" @change="() => handleTagSelection(tag)" class="mr-2 cursor-pointer accent-blue-500" />
        <span class="text-sm text-gray-800">{{ tag }}</span>
      </label>
    </div>
    <div class="flex items-center">
      <input
        type="text"
        v-model="newTagInput"
        placeholder="Add a new tag"
        class="w-full p-2 border border-gray-300 rounded-md mr-3"
      />
      <button
        @click="addNewTag"
        class="bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500"
      >
        Add Tag
      </button>
    </div>
  </div>
</template>

<style scoped>
</style>
