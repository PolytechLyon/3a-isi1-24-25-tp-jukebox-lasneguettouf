<script setup>
import AddTrack from './AddTrack.vue';
import { ref } from "vue";

// Mode de lecture sélectionné
const selectedMode = ref("repeatList");

// Liste des pistes
const tracks = ref([]);

// Fonction pour ajouter une piste
function addTrack(track) {
  if (track.trim() !== "") {
    tracks.value.unshift(track); // Ajoute la piste au début de la liste
  }
}
</script>

<template>
  <h1>Jukebox</h1>
  <hr>
  <h2>Player</h2>
  <p>Choose a track to play:</p>

  <!-- Section Player -->
  <div>
    <fieldset>
      <legend>Playback mode</legend>
      <label>
        <input
          type="radio"
          name="playbackMode"
          value="repeatList"
          v-model="selectedMode"
        />
        Repeat list
      </label>
      <label>
        <input
          type="radio"
          name="playbackMode"
          value="repeatTrack"
          v-model="selectedMode"
        />
        Repeat track
      </label>
      <label>
        <input
          type="radio"
          name="playbackMode"
          value="noRepeat"
          v-model="selectedMode"
        />
        Don't repeat
      </label>
    </fieldset>
  </div>
  <hr>

  <!-- Section Playlist -->
  <h2>Playlist</h2>
  <p v-if="!tracks.length">Add a new tracklist to the playlist:</p>
  <ul v-else>
    <li v-for="(track, index) in tracks" :key="index">
      <p>{{ track }}</p>
    </li>
  </ul>

  <hr>
  <h2>New track</h2>
  <AddTrack @add-track="addTrack" />
</template>

