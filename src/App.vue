<template>
  <Header />
  <div
    class="container min-vh-100 d-flex align-items-center justify-content-center app-bg"
  >
    <div class="w-100" style="max-width: 700px; height: 80vh bg-info">
      <div class="quiz-card">
        <div class="card-body p-4">
          <div class="d-flex align-items-center justify-content-between mb-4">
            <div>
              <p class="text-black fw-bold mb-1">
                Question {{ currentQuestionIndex + 1 }}
              </p>
            </div>
            <div class="text-end text-secondary">
              <span>sur {{ questions.length }}</span>
            </div>
          </div>
          <div class="mb-4">
            <h2 class="fw-bold text-black mb-3" style="font-size: 1.5rem">
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
</template>

<script setup>
import Header from "./Header.vue";
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
</script>

<style scoped></style>
