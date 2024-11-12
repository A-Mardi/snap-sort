<script lang="ts" setup>
import { ref } from 'vue';

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
  <div class="filter-component" style="margin-bottom: 20px;">
    <h3>Filter by Tags:</h3>
    <div>
      <label v-for="tag in availableTags" :key="tag" style="margin-right: 10px;">
        <input type="checkbox" :value="tag" @change="() => handleTagSelection(tag)" />
        {{ tag }}
      </label>
    </div>
    <div style="margin-top: 10px;">
      <input type="text" v-model="newTagInput" placeholder="Add a new tag" />
      <button @click="addNewTag" style="margin-left: 5px;">Add Tag</button>
    </div>
  </div>
</template>

<style scoped>
</style>