<script setup>
import { ref } from 'vue';

// État local pour l'URL
const trackUrl = ref("");

// Événement pour transmettre la piste ajoutée au parent
const emit = defineEmits(["add-track"]);

// Ajouter une piste
function addTrack() {
  if (trackUrl.value.trim() !== "") {
    emit("add-track", trackUrl.value); // Émet l'URL de la piste au parent
    trackUrl.value = ""; // Réinitialise le champ
  } else {
    alert("Please provide a valid URL.");
  }
}
</script>

<template>
  <div>
    <input
      type="text"
      placeholder="Provide a track URL"
      v-model="trackUrl"
    />
    <button @click="addTrack">Add Track</button>
  </div>
</template>

<style scoped>
input {
  padding: 5px;
  width: 300px;
}

button {
  margin-left: 10px;
  padding: 5px 10px;
}
</style>
