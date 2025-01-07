<script setup>
import { ref, defineProps, watch } from 'vue';

// Reçoit la piste en cours
defineProps({
    currentTrack: {
        type: Object,
        default: null,
    },
});

// État pour le mode de lecture
const selectedMode = ref("noRepeat");

// Référence à l'élément audio
const audioElement = ref(null);

// Surveille la fin de la piste pour répéter si "Repeat track" est activé
function handleTrackEnd() {
    if (selectedMode.value === "repeatTrack" && audioElement.value) {
        audioElement.value.currentTime = 0; // Revient au début
        audioElement.value.play(); // Relance la lecture
    }
}
</script>

<template>
    <div>
        <div v-if="currentTrack">
            <p><strong>Now playing:</strong> {{ currentTrack.name }}</p>
            <audio
                ref="audioElement"
                controls
                autoplay
                @ended="handleTrackEnd"
            >
                <source :src="currentTrack.url" type="audio/mpeg" />
                Your browser does not support the audio element.
            </audio>
        </div>
        <div v-else>
            <p>No track selected.</p>
        </div>
    </div>
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
</template>
