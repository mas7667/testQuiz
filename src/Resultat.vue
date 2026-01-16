<template>
  <!-- Conteneur principal pour l'affichage des résultats -->
  <div
    class="result-bg d-flex flex-column align-items-center justify-content-center min-vh-100"
  >
    <div class="result-card text-center p-5">
      <!-- Affiche le score final en cercle -->
      <div class="mb-4">
        <div class="result-score-circle mb-2">
          <span class="result-score">{{ score }}</span>
          <span class="result-outof"> sur {{ total }}</span>
        </div>
      </div>

      <!-- Titre et description du résultat selon la performance -->
      <h2 class="fw-bold text-white mb-2">{{ resultTitle }}</h2>
      <p class="text-secondary mb-4">{{ resultDesc }}</p>
      <div class="mb-4">
        <label class="text-secondary mb-1">Précision</label>
        <div class="progress" style="height: 8px; background: #232946">
          <div
            class="progress-bar bg-gradient"
            role="progressbar"
            :style="{ width: accuracy + '%' }"
          ></div>
        </div>
        <span class="text-white mt-1 d-block">{{ accuracy }}%</span>
      </div>

      <!-- Bouton pour recommencer le quiz -->
      <button class="btn btn-light btn-lg mt-2 next-btn" @click="playAgain">
        <span class="me-2">&#8635;</span> Rejouer
      </button>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

// Props reçues du composant parent (score et nombre total de questions)

const props = defineProps({
  score: Number,
  total: Number,
});
const emit = defineEmits(["play-again"]);

const accuracy = Math.round((props.score / props.total) * 100);

// Détermine le titre du résultat en fonction du score

const resultTitle =
  props.score === props.total
    ? "Mission Accomplie"
    : props.score === 0
    ? "Mission Échouée"
    : props.score > props.total / 2
    ? "Bien Joué !"
    : "Continuez vos efforts !";

// Détermine la description du résultat en fonction du score

const resultDesc =
  props.score === props.total
    ? "Score parfait ! Vous connaissez vraiment le sujet."
    : props.score === 0
    ? "Il est temps de retourner à l'école de foot."
    : props.score > props.total / 2
    ? "Bravo ! Vous avez réussi la plupart des questions."
    : "Vous pouvez mieux faire la prochaine fois !";

// Émet un événement pour recommencer le quiz

function playAgain() {
  emit("play-again");
}
</script>

<style scoped>
.result-bg {
  background: linear-gradient(135deg, #232946 0%, #1a223f 100%);
}
.result-card {
  background: rgba(30, 34, 54, 0.98);
  border-radius: 20px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  border: 1px solid rgba(255, 255, 255, 0.08);
  min-width: 350px;
  max-width: 400px;
}
.result-trophy {
  font-size: 2.5rem;
  color: #ffd700;
}
.result-score-circle {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background: rgba(44, 51, 73, 0.7);
  margin: 0 auto 10px auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border: 2px solid #6366f1;
}
.result-score {
  font-size: 2.5rem;
  font-weight: bold;
  color: #fff;
}
.result-outof {
  font-size: 1rem;
  color: #aaa;
}
.next-btn {
  border-radius: 10px;
  font-weight: 600;
  color: #232946;
  box-shadow: 0 2px 8px rgba(44, 51, 73, 0.08);
}
.progress-bar.bg-gradient {
  background: linear-gradient(90deg, #a78bfa 0%, #6366f1 100%);
}
</style>
