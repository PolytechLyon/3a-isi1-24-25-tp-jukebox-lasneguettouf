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
    currentTrack.value = track; // Définit la piste sélectionnée comme la piste en cours
}
</script>

<template>
    <h1>Jukebox</h1>
    <hr>
    <h2>Player</h2>
    <!-- Passe la piste en cours de lecture au Player -->
    <Player :currentTrack="currentTrack" />
    <hr>
    <h2>Playlist</h2>
    <!-- Passe les pistes et écoute l'événement play-track -->
    <Playlist :tracks="tracks" @play-track="handlePlayTrack" />
    <hr>
    <h2>New track</h2>
    <AddTrack @add-track="handleAddTrack" />
</template>
