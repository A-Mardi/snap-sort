<script lang="ts" setup>
import { ref } from 'vue';

    const photos = ref<{ file: File; url: string }[]>([]);

    const handleFileInput = (event: Event) => {
      const target = event.target as HTMLInputElement;
      if (target.files) {
        const newPhotos = Array.from(target.files).map((file) => ({
          file,
          url: URL.createObjectURL(file),
        }));
        photos.value = [...photos.value, ...newPhotos];
      }
    };
</script>

<template>
  <div>
    <input type="file" multiple @change="handleFileInput" />
    <div v-if="photos.length" style="margin-top: 20px;">
      <h3>Test Uploaded Photos:</h3>
      <ul style="list-style-type: none; padding: 0; display: flex; flex-wrap: wrap; gap: 10px;">
        <li v-for="(photo, index) in photos" :key="index" style="display: flex; flex-direction: column; align-items: center;">
          <img :src="photo.url" :alt="photo.file.name" style="border: 1px solid #ccc; padding: 5px; width: 150px; height: auto;" />
          <span style="margin-top: 5px; font-size: 0.9em;">{{ photo.file.name }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>
