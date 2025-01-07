<script setup>
import AddTrack from './AddTrack.vue';
import Player from './Player.vue';
import Playlist from './Playlist.vue';
import { ref } from "vue";

const tracks = ref([]);

function handleAddTrack(track) {
    tracks.value.push(track);
}

</script>

<template>
    <h1>Jukebox</h1>
    <hr>
    <h2>Player</h2>
    <Player/>
    <hr>
    <h2>Playlist</h2>
    <Playlist :tracks="tracks" />
    <hr>
    <h2>New track</h2>
    <AddTrack @add-track="handleAddTrack" />
</template>
