<script setup>
import { ref } from 'vue';
const selectedTrack = ref(null);
const selectedMode = ref("repeatList");
</script>

<template>
    <p>Choose a track to play.</p>
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
</template>
