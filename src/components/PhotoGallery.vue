<script lang="ts" setup>
import { ref, defineProps, computed } from 'vue';

const props = defineProps<{ filterTags: string[] }>();
const photos = ref<{ file: File; url: string; tags: string[] }[]>([]);

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
    props.filterTags.some(tag => photo.tags.includes(tag))
  );
});
</script>

<template>
  <div>
    <input type="file" multiple @change="handleFileInput" />

    <div v-if="filteredPhotos.length" class="photo-gallery" style="margin-top: 20px;">
      <h3>Uploaded Photos:</h3>
      <ul style="list-style-type: none; padding: 0; display: flex; flex-wrap: wrap; gap: 10px;">
        <li v-for="(photo, index) in filteredPhotos" :key="index" style="display: flex; flex-direction: column; align-items: center;">
          <img :src="photo.url" :alt="photo.file.name" style="border: 1px solid #ccc; padding: 5px; width: 150px; height: auto;" />
          <span style="margin-top: 5px; font-size: 0.9em;">{{ photo.file.name }}</span>
          <button @click="handleDelete(index)" style="margin-top: 10px; padding: 5px 10px; background-color: red; color: white; border: none; cursor: pointer;">
            Delete
          </button>
          <div class="tagging-component" style="margin-top: 20px;">
            <h3>Tags for this Photo:</h3>
            <div>
              <span v-for="tag in photo.tags" :key="tag" style="margin-right: 10px;">
                {{ tag }}
              </span>
            </div>
            <input type="text" v-model="newTags[index]" placeholder="Add a tag" />
            <button @click="addTag(index)" style="margin-left: 5px;">Add Tag</button>
          </div>
        </li>
      </ul>
    </div>
    <div v-else>
      <p>No photos to display.</p>
    </div>
  </div>
</template>

<style scoped>
</style>
