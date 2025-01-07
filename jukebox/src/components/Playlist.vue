<script setup>
import { defineProps } from "vue";

defineProps({
    tracks: {
        type: Array,
        default: () => [],
    },
});

</script>

<template>
    <p v-if="!tracks.length">Add a new tracklist to the playlist:</p>
    <ul v-else>
        <li v-for="(track, index) in tracks" :key="index">
        <p>{{ track }}</p>
        </li>
    </ul>
</template>
