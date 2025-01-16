<script setup>
import AddTrack from './AddTrack.vue';
import Player from './Player.vue';
import Playlist from './Playlist.vue';
import { ref } from "vue";

const tracks = ref([]);
const currentTrack = ref(null); // Piste en cours de lecture

// Ajouter une piste
function handleAddTrack(track) {
  tracks.value.push(track);
}

// Jouer une piste
function handlePlayTrack(track) {
  currentTrack.value = null; // Réinitialisation
  setTimeout(() => {
    currentTrack.value = JSON.parse(JSON.stringify(track)); // Création d'une nouvelle référence
  }, 100);
}

// Recevoir une piste mise à jour (via Repeat List)
function handleUpdateCurrentTrack(updatedTrack) {
  console.log("Updated current track:", updatedTrack);
  currentTrack.value = updatedTrack;
}
</script>

<template>
  <h1>Jukebox</h1>
  <hr>
  <h2>Player</h2>
  <!-- Passe la piste en cours et écoute l'événement pour la piste suivante -->
  <Player
    :currentTrack="currentTrack"
    :tracks="tracks"
    @update:currentTrack="handleUpdateCurrentTrack"
  />
  <hr>
  <h2>Playlist</h2>
  <!-- Passe les pistes et gère l'événement play -->
  <Playlist :tracks="tracks" @play-track="handlePlayTrack" />
  <hr>
  <h2>New track</h2>
  <AddTrack @add-track="handleAddTrack" />
</template>
