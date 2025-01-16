<script setup>
import { ref, defineProps, toRefs, watch, defineEmits } from 'vue';

const props = defineProps({
  currentTrack: {
    type: Object,
    default: null,
  },
  tracks: {
    type: Array,
    default: () => [],
  },
});

const { tracks, currentTrack } = toRefs(props);
const emit = defineEmits(["update:currentTrack"]);

const audioElement = ref(null);

const currentTime = ref(0);
const duration = ref(0);
const selectedMode = ref("noRepeat"); 

// Detection du changement de piste
watch(
  () => props.currentTrack?.url,
  (newUrl) => {
    if (newUrl) {
      console.log("Nouvelle piste reçue :", props.currentTrack);
      if (audioElement.value) {
        audioElement.value.src = newUrl;  
        audioElement.value.load();  
        audioElement.value.play().catch((err) =>
          console.error("Erreur de lecture :", err)
        ); 
      }
    } else {
      console.log("Aucune piste reçue ou champ 'url' manquant");
    }
  },
  { immediate: true }
);

// Lecture
function playAudio() {
  if (audioElement.value) {
    audioElement.value.play();
  }
}

// Pause
function pauseAudio() {
  if (audioElement.value) {
    audioElement.value.pause();
  }
}

// Arrêt
function stopAudio() {
  if (audioElement.value) {
    audioElement.value.pause();
    audioElement.value.currentTime = 0;
  }
}

// Gestion de la fin de la piste
function handleTrackEnd() {
  console.log("Piste finie, mode séléctionné:", selectedMode.value);

  const currentIndex = tracks.value.findIndex(
    (track) => track.name === currentTrack.value?.name
  );

  if (selectedMode.value === "repeatTrack") {
    if (audioElement.value) {
      audioElement.value.currentTime = 0;
      audioElement.value.play();
    }
  } else if (selectedMode.value === "repeatList") {
    console.log("Passage à la piste suivante...");
    if (tracks.value.length > 0) {
      const nextIndex = (currentIndex + 1) % tracks.value.length;
      emit("update:currentTrack", { ...tracks.value[nextIndex] });
    }
  } else {
    console.log("Pas de répétition.");
  }
}

// Mise à jour du temps actuel et de la durée de la piste
function updateProgress() {
  if (audioElement.value) {
    currentTime.value = audioElement.value.currentTime;
    duration.value = audioElement.value.duration || 0;
  }
}

// Actuallistation de la position de lecture
function seek(event) {
  if (audioElement.value) {
    const newTime = (event.target.value / 100) * duration.value;
    audioElement.value.currentTime = newTime;
    currentTime.value = newTime;
  }
}

// Formatage du temps en minute seconde
function formatTime(seconds) {
  const mins = Math.floor(seconds / 60);
  const secs = Math.floor(seconds % 60);
  return `${mins}:${secs.toString().padStart(2, '0')}`;
}
</script>

<template>
  <div>
    <div v-if="currentTrack">
      <p><strong>Now playing:</strong> {{ currentTrack.name }}</p>
      <audio
        ref="audioElement"
        :src="currentTrack?.url"
        autoplay
        @ended="handleTrackEnd"
        @timeupdate="updateProgress"
      ></audio>
      <input
        type="range"
        min="0"
        max="100"
        :value="(currentTime / duration) * 100 || 0"
        @input="seek"
      />
      <div>
        <span>{{ formatTime(currentTime) }}</span> /
        <span>{{ formatTime(duration) }}</span>
      </div>

      <button @click="playAudio">Play</button>
      <button @click="pauseAudio">Pause</button>
      <button @click="stopAudio">Stop</button>
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
