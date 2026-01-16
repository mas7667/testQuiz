<template>
  <Header v-if="!showResult" />
  <div
    v-if="!showResult"
    class="container min-vh-100 d-flex align-items-center justify-content-center app-bg"
  >
    <div class="w-100" style="max-width: 700px; height: 80vh">
      <div class="quiz-card">
        <div class="card-body p-4">
          <div class="d-flex align-items-center justify-content-between mb-4">
            <div>
              <p class="text-white fw-bold mb-1">
                Question {{ currentQuestionIndex + 1 }}
              </p>
            </div>
            <div class="text-end text-secondary">
              <span>sur {{ questions.length }}</span>
            </div>
          </div>
          <div class="mb-4">
            <h2 class="fw-bold text-white mb-3" style="font-size: 1.5rem">
              {{ currentQuestion.question }}
            </h2>
          </div>
          <div class="mb-4">
            <div class="d-grid gap-3">
              <button
                v-for="(choice, index) in currentQuestion.choices"
                :key="index"
                @click="selectAnswer(index)"
                :class="getChoiceClass(index)"
                style="font-size: 1.1rem"
              >
                <span class="me-3 fw-bold">{{ choice }}</span>
                <span
                  v-if="
                    selectedAnswer !== null &&
                    index === currentQuestion.correctAnswer
                  "
                  class="ms-auto text-success fw-bold"
                  >&#10003;</span
                >
                <span
                  v-else-if="
                    selectedAnswer !== null &&
                    index === selectedAnswer &&
                    selectedAnswer !== currentQuestion.correctAnswer
                  "
                  class="ms-auto text-danger fw-bold"
                  >&#10007;</span
                >
              </button>
            </div>
          </div>
          <button
            @click="nextQuestion"
            :disabled="selectedAnswer === null"
            class="btn btn-light w-100 btn-lg mt-2 next-btn"
          >
            {{ isLastQuestion ? "Voir le résultat" : "Question suivante" }}
            <span class="ms-2">&rarr;</span>
          </button>
        </div>
      </div>
    </div>
  </div>
  <Resultat
    v-else
    :score="score"
    :total="questions.length"
    @play-again="playAgain"
  />
</template>

<script setup>
import Header from "./composants/Header.vue";
import Resultat from "./composants/Resultat.vue";
import { ref, computed } from "vue";
import questions from "./questions.json";

const currentQuestionIndex = ref(0);
const selectedAnswer = ref(null);
const score = ref(0);
const showResult = ref(false);

const currentQuestion = computed(() => questions[currentQuestionIndex.value]);
const isLastQuestion = computed(
  () => currentQuestionIndex.value === questions.length - 1
);

function getChoiceClass(index) {
  if (selectedAnswer.value === null) {
    return "btn btn-lg btn-dark text-start d-flex align-items-center choice-btn";
  }
  if (index === currentQuestion.value.correctAnswer) {
    return "btn btn-lg btn-success text-start d-flex align-items-center choice-btn border-success";
  }
  if (index === selectedAnswer.value) {
    return "btn btn-lg btn-danger text-start d-flex align-items-center choice-btn border-danger";
  }
  return "btn btn-lg btn-secondary text-start d-flex align-items-center choice-btn";
}

function selectAnswer(index) {
  selectedAnswer.value = index;
}

function nextQuestion() {
  if (selectedAnswer.value === currentQuestion.value.correctAnswer) {
    score.value++;
  }
  selectedAnswer.value = null;
  if (!isLastQuestion.value) {
    currentQuestionIndex.value++;
  } else {
    showResult.value = true;
  }
}

function playAgain() {
  showResult.value = false;
  currentQuestionIndex.value = 0;
  score.value = 0;
}
</script>

<style scoped>
.app-bg {
  background: transparent !important;
}
.quiz-card {
  background: rgba(30, 34, 54, 0.98);
  border-radius: 20px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  border: 1px solid rgba(255, 255, 255, 0.08);
}
.choice-btn {
  border-radius: 12px !important;
  font-weight: 500;
  transition: all 0.2s;
  box-shadow: none;
  background: rgba(44, 51, 73, 0.7);
  color: #fff;
}
.btn-success.choice-btn {
  background: rgba(44, 73, 51, 0.7) !important;
  color: #b6f7c1 !important;
  border: 2px solid #2ecc71 !important;
}
.btn-danger.choice-btn {
  background: rgba(73, 44, 51, 0.7) !important;
  color: #f7b6b6 !important;
  border: 2px solid #e74c3c !important;
}
.btn-secondary.choice-btn {
  background: rgba(44, 51, 73, 0.3) !important;
  color: #aaa !important;
  border: 2px solid #232946 !important;
}
.btn-dark.choice-btn {
  background: rgba(44, 51, 73, 0.7) !important;
  color: #fff !important;
  border: 2px solid #232946 !important;
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
