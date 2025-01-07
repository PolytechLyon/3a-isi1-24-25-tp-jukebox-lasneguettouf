<script setup>
import { ref, computed } from "vue";

const trackMethod = ref("url");
const url = ref("");
const file = ref(null);

const isButtonDisabled = computed(() => {
  return trackMethod.value === "url" ? !url.value : !file.value;
});

function onFileSelect(event) {
  file.value = event.target.files[0];
}
</script>

<template>
    <div>
        <label for="add-track">Add track</label>
        <select id="add-track" v-model="trackMethod">
        <option value="url">By URL</option>
        <option value="file">By File</option>
        </select>
        <input
        v-if="trackMethod === 'url'"
        type="text"
        placeholder="Provide URL"
        v-model="url"
        />
        <input v-else type="file" @change="onFileSelect" />
        <button :disabled="isButtonDisabled">
        {{ trackMethod === 'url' ? 'Add by URL' : 'Add by File' }}
        </button>
    </div>
</template>


