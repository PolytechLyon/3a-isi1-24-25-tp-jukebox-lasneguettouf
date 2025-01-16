<script setup>
import { defineProps } from "vue";

defineProps({
    tracks: {
        type: Array,
        default: () => [],
    },
});

// Transmission des évênements au parent
const emit = defineEmits(["play-track", "delete-track"]);

// Joueur une piste
function playTrack(track) {
    console.log("Track selected:", track);
    emit("play-track", track);
}


// Suppression d'une piste
function deleteTrack(index) {
    emit("delete-track", index);
}
</script>

<template>
    <p v-if="!tracks.length">Add a new tracklist to the playlist:</p>
    <ul v-else>
        <li v-for="(track, index) in tracks" :key="index">
            <div class="track-item">
                <p class="track-name">{{ track.name }}</p>
                <button @click="playTrack(track)" class="btn-play">Play</button>
                <button @click="deleteTrack(index)" class="btn-delete">Delete</button>
            </div>
        </li>
    </ul>
</template>



<style scoped>

ul {
    list-style-type: none;
    padding: 0;
}

li {
    padding: 10px;
    margin: 5px 0;
    border: 1px solid #ccc;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.track-item {
    display: flex;
    align-items: center;
    width: 100%;
}

.track-name {
    flex: 1;
    margin-right: 10px;
}

button {
    padding: 5px 10px;
    margin-left: 5px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.btn-play {
    background-color: #4caf50;
    color: white;
}

.btn-play:hover {
    background-color: #45a049;
}

.btn-delete {
    background-color: #f44336;
    color: white;
}

.btn-delete:hover {
    background-color: #e53935;
}

</style>
