<template>
  <v-container max-width="700">
    <!-- Données de l'exercice -->
    <exercice-objectifs number="6"/>
    <!-- Zone de travail pour l'exercice -->
    <div class="exe-zone">
      <h2>Zone d'exercice</h2>
      <v-card
        class="mx-auto my-6 pa-2"
        max-width="500"
      >
        <v-text-field
          v-model="newTask"
          label="Nouvelle tâche"
          clearable
          @keyup.enter="addTask"
        >
          <template v-slot:append-inner>
            <v-btn @click="addTask">Ajouter</v-btn>
          </template>
        </v-text-field>

        <div v-if="trierTasks.length > 0">
          <v-card-title v-for="(task, index) in trierTasks" :key="index">
            {{ index }} - {{ task.title }}
          </v-card-title>
        </div>

        <v-card-subtitle v-else>
          Il n'y a pas de tâches... chanceux ! 😄
        </v-card-subtitle>

        <v-list>
          <v-list-item v-for="(task, index) in trierTasks" :key="index">
            <template v-slot:prepend>
              <v-list-item-action start>
                <v-checkbox-btn v-model="task.completed"/>
              </v-list-item-action>
            </template>

            <v-list-item-title :class="{done: task.completed}">
              {{ task.title }}
            </v-list-item-title>

            <v-list-item-subtitle>
              Créé le {{ new Date(task.date).toLocaleDateString() }}
              à {{ new Date(task.date).toLocaleTimeString() }}
            </v-list-item-subtitle>
          </v-list-item>
        </v-list>
      </v-card>
    </div>
  </v-container>
</template>

<script setup>
// Importation du composant ExerciceObjectifs
import ExerciceObjectifs from "@/components/ExerciceObjectifs.vue";
// Importation de la fonction réactive ref
import {ref, computed, watch} from 'vue';

// Tableau réactif de tâches
const tasks = ref([
  {
    "title": "Acheter du Lait",
    "completed": false,
    "date": 1738162351961
  },
  {
    "title": "Nettoyer le four",
    "completed": false,
    "date": 1737978751912
  },
  {
    "title": "Acheter de l'aspirine",
    "completed": true,
    "date": 1737856351933
  }
]);
// Nouvelle tâche à ajouter
const newTask = ref("");

/**
 * Fonction qui ajoute une nouvelle tâche à la liste.
 */
function addTask() {
  // Ajout de la nouvelle tâche
  tasks.value.push({
    "title": newTask.value,
    "completed": false,
    "date": Date.now() // Date actuelle au format timestamp
  });
  // Réinitialisation de la saisie
  newTask.value = "";
}

// Propriété calculée pour trier les tâches par date (la plus récente en premier)
const trierTasks = computed(() => {
  return tasks.value.sort((a, b) => a.date - b.date);
});

// Watcher pour surveiller la saisie utilisateur et supprimer les tâches si "delete" est saisi
watch(newTask, (newValue) => {
  if (newValue.toLowerCase() === 'delete') {
    tasks.value = []; // Réinitialiser la liste des tâches
    newTask.value = ''; // Réinitialiser la saisie de texte
  }
});

</script>

<style scoped lang="sass">
.done
  text-decoration: line-through
</style>
