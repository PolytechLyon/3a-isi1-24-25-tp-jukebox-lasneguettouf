<script setup>
import { ref, onMounted, watch } from "vue";
import AddTrack from "./AddTrack.vue";
import Player from "./Player.vue";
import Playlist from "./Playlist.vue";

// Création de la liste des pistes et de la piste en cours de lecture
const tracks = ref([]);
const currentTrack = ref(null);

// Sauvegarde la playlist dans localStorage
// Sauvegarde uniquement les pistes qui ont une URL valide et ne sont pas des fichiers locaux
function saveTracks() {
    const urlTracks = tracks.value.filter(track => !track.url.startsWith("blob:"));
    localStorage.setItem("jukebox_tracks", JSON.stringify(urlTracks));
}


// Charge la playlist depuis localStorage au démarrage
onMounted(() => {
    const savedTracks = localStorage.getItem("jukebox_tracks");
    if (savedTracks) {
        tracks.value = JSON.parse(savedTracks);
    }
});

// Surveiller les changements dans la playlist pour la sauvegarder automatiquement
watch(tracks, saveTracks, { deep: true });

// Ajouter une piste à la playlist
function handleAddTrack(track) {
    tracks.value.push(track);
}

// Jouer une piste
function handlePlayTrack(track) {
    currentTrack.value = track;
}

// Supprimer une piste
function handleDeleteTrack(index) {
    if (tracks.value[index]?.url === currentTrack.value?.url) {
        currentTrack.value = null; // Stop la lecture si la piste supprimée est en cours
    }
    tracks.value.splice(index, 1);
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
  <Playlist :tracks="tracks" @play-track="handlePlayTrack" @delete-track="handleDeleteTrack" />
  <hr>
  <h2>New track</h2>
  <AddTrack @add-track="handleAddTrack" />
</template>
