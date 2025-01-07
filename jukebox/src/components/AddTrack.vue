<script setup>
import { ref } from 'vue';

// État local pour le type d'ajout et les données
const addTrackMethod = ref("url"); // Par défaut : URL
const trackUrl = ref("");
const trackFile = ref(null);

// Événement pour transmettre la piste ajoutée au parent
const emit = defineEmits(["add-track"]);

// Ajouter une piste
function addTrack() {
    if (addTrackMethod.value === "url") {
        // Ajout via URL
        if (trackUrl.value.trim() !== "") {
            emit("add-track", trackUrl.value); // Émet l'URL de la piste au parent
            trackUrl.value = ""; // Réinitialise le champ
        } else {
            alert("Please provide a valid URL.");
        }
    } else if (addTrackMethod.value === "file") {
        // Ajout via fichier
        if (trackFile.value) {
            // Vérifie que le fichier est bien un .mp3
            if (trackFile.value.type === "audio/mpeg") {
                emit("add-track", `File: ${trackFile.value.name}`); // Émet le nom du fichier au parent
                trackFile.value = null; // Réinitialise le fichier
            } else {
                alert("Only .mp3 files are allowed.");
                trackFile.value = null; // Réinitialise le fichier si invalide
            }
        } else {
            alert("Please select a file.");
        }
    }
}

// Gestion du changement de fichier
function onFileChange(event) {
    trackFile.value = event.target.files[0];
}
</script>

<template>
<div>
    <!-- Sélection du type d'ajout -->
    <label for="addTrackMethod">Add track by:</label>
    <select id="addTrackMethod" v-model="addTrackMethod">
        <option value="url">URL</option>
        <option value="file">File</option>
    </select>

    <!-- Ajout par URL -->
    <div v-if="addTrackMethod === 'url'">
        <input
            type="text"
            placeholder="Provide a track URL"
            v-model="trackUrl"
        />
    </div>

    <!-- Ajout par fichier -->
    <div v-else-if="addTrackMethod === 'file'">
        <input
            type="file"
            accept=".mp3"
            @change="onFileChange"
        />
    </div>

    <!-- Bouton d'ajout -->
    <button @click="addTrack">Add Track</button>
</div>
</template>
