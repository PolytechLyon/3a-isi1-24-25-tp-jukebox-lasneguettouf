<script setup>
import { ref } from 'vue';

// État local pour le type d'ajout et les données
const addTrackMethod = ref("file"); // Par défaut : fichier
const trackFile = ref(null);

// Événement pour transmettre la piste ajoutée au parent
const emit = defineEmits(["add-track"]);

// Ajouter une piste
function addTrack() {
    if (addTrackMethod.value === "file") {
        if (trackFile.value) {
            // Créer une URL Blob pour le fichier
            const fileUrl = URL.createObjectURL(trackFile.value);
            emit("add-track", { name: trackFile.value.name, url: fileUrl });
            trackFile.value = null; // Réinitialise le champ fichier
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
    <label for="addTrackMethod">Add track by:</label>
    <select id="addTrackMethod" v-model="addTrackMethod" disabled>
        <option value="file">Via file upload</option>
    </select>

    <div>
        <input
            type="file"
            @change="onFileChange"
            accept=".mp3"
        />
    </div>

    <button @click="addTrack" :disabled="!trackFile">Add uploaded file</button>
</div>
</template>
