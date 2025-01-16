<script setup>
import { ref, defineProps, toRefs, watch, defineEmits } from 'vue';

// Propriétés reçues du parent
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

// Rend `tracks` et `currentTrack` réactifs
const { tracks, currentTrack } = toRefs(props);
const emit = defineEmits(["update:currentTrack"]); // Permet de notifier le parent des changements

// Référence à l'élément audio
const audioElement = ref(null);

// Suivi de la progression
const currentTime = ref(0);
const duration = ref(0);
const selectedMode = ref("noRepeat"); // Mode par défaut : pas de répétition

// Watch pour détecter les changements de piste
watch(() => props.currentTrack?.url, (newUrl) => {
    if (newUrl) {
        console.log("Nouvelle piste reçue :", props.currentTrack);
        if (audioElement.value) {
            audioElement.value.src = newUrl;  
            audioElement.value.load();  
            audioElement.value.play().catch(err => console.error("Erreur de lecture :", err)); 
        }
    } else {
        console.log("Aucune piste reçue ou champ 'url' manquant");
    }
}, { immediate: true });

// Lecture
function playAudio() {
    if (audioElement.value) {
        console.log("Playing track:", currentTrack.value);
        audioElement.value.play();
    }
}

// Pause
function pauseAudio() {
  if (audioElement.value) {
    audioElement.value.pause();
  }
}

// Arrêt (pause + retour au début)
function stopAudio() {
  if (audioElement.value) {
    audioElement.value.pause();
    audioElement.value.currentTime = 0;
  }
}

// Gestion de la fin de la piste
function handleTrackEnd() {
  console.log("Track ended. Mode selected:", selectedMode.value);

  const currentIndex = tracks.value.findIndex(track => track === currentTrack.value);

  if (selectedMode.value === "repeatTrack") {
    console.log("Repeating track...");
    if (audioElement.value) {
      audioElement.value.currentTime = 0;
      audioElement.value.play();
    }
  } else if (selectedMode.value === "repeatList") {
    console.log("Going to next track...");
    if (tracks.value.length > 0) {
      const nextIndex = (currentIndex + 1) % tracks.value.length;
      emit("update:currentTrack", { ...tracks.value[nextIndex] }); // Met à jour la piste via le parent
    }
  } else {
    console.log("No repeat, stopping playback.");
  }
}

// Met à jour le temps actuel et la durée de la piste
function updateProgress() {
  if (audioElement.value) {
    currentTime.value = audioElement.value.currentTime;
    duration.value = audioElement.value.duration || 0;
  }
}

// Change la position de lecture
function seek(event) {
  if (audioElement.value) {
    const newTime = (event.target.value / 100) * duration.value;
    audioElement.value.currentTime = newTime;
    currentTime.value = newTime;
  }
}

// Formate le temps en minutes et secondes
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

      <!-- Barre de progression -->
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
