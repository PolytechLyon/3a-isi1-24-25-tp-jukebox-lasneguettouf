<script setup>
import { ref } from 'vue';

// État local pour choisir la méthode d'ajout
const addTrackMethod = ref("file"); // "file" ou "url"
const trackFile = ref(null);
const trackUrl = ref(""); // Stocke l'URL saisie

// Événement pour transmettre la piste ajoutée au parent
const emit = defineEmits(["add-track"]);

// Ajouter une piste
function addTrack() {
    if (addTrackMethod.value === "file") {
        if (trackFile.value) {
            const fileUrl = URL.createObjectURL(trackFile.value);
            emit("add-track", { name: trackFile.value.name, url: fileUrl });
            trackFile.value = null;
        } else {
            alert("Please select a file.");
        }
    } else if (addTrackMethod.value === "url") {
        if (trackUrl.value.trim() === "") {
            alert("Please enter a valid URL.");
            return;
        }
        emit("add-track", { name: trackUrl.value.split('/').pop(), url: trackUrl.value });
        trackUrl.value = "";
    }
}

// Gestion du changement de fichier
function onFileChange(event) {
    trackFile.value = event.target.files[0];
}
</script>

<template>
    <div>
        <label for="addTrackMethod">Add track by:</label>
        <select id="addTrackMethod" v-model="addTrackMethod">
            <option value="file">Via file upload</option>
            <option value="url">Via URL</option>
        </select>

        <div v-if="addTrackMethod === 'file'">
            <input type="file" @change="onFileChange" accept=".mp3" />
            <button @click="addTrack" :disabled="!trackFile">Add uploaded file</button>
        </div>

        <div v-if="addTrackMethod === 'url'">
            <input type="text" v-model="trackUrl" placeholder="Enter track URL" />
            <button @click="addTrack" :disabled="!trackUrl.trim()">Add URL track</button>
        </div>
    </div>
</template>
