<template>
  <div class="cijferlijst-container">
    <h1>Cijferlijst</h1>
    <table>
      <thead>
      <tr>
        <th>Examen</th>
        <th>Cijfer</th>
        <th>Actie</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(exam, index) in examens" :key="index" :class="{ 'failed': exam.grade < 5.5, 'passed': exam.grade >= 5.5 }">
        <td>{{ exam.name }}</td>
        <td>
          <input type="number" v-model.number="exam.grade" min="0" max="10" step="0.1" @input="checkGrade(exam)">
        </td>
        <td>
          <button @click="removeExam(index)">❌</button>
        </td>
      </tr>
      </tbody>
    </table>

    <div class="controls">
      <input v-model="newExam" placeholder="Nieuw examen">
      <button @click="addExam">Toevoegen</button>
    </div>

    <p><strong>Gemiddeld:</strong> {{ averageGrade.toFixed(1) }}</p>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

const examens = ref([
  { name: "Webdevelopment", grade: 7.5 },
  { name: "Mobile Application Development", grade: 4.5 },
  { name: "Stage 1", grade: 6.1 }
]);

const newExam = ref("");

const addExam = () => {
  if (newExam.value.trim() !== "") {
    examens.value.push({ name: newExam.value, grade: 1.0 });
    newExam.value = "";
  }
};

const removeExam = (index: number) => {
  examens.value.splice(index, 1);
};

const averageGrade = computed(() => {
  if (examens.value.length === 0) return 0;
  const total = examens.value.reduce((sum, exam) => sum + exam.grade, 0);
  return total / examens.value.length;
});

const checkGrade = (exam: { grade: number }) => {
  if (exam.grade > 10) {
    exam.grade = 10;
  }
};

</script>

<style scoped>
.cijferlijst-container {
  max-width: 500px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background: #f9f9f9;
  color: black;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 8px;
  text-align: left;
  border: 1px solid #ddd;
}

.failed {
  background-color: #f8d7da;
}

.passed {
  background-color: #d4edda;
}

.controls {
  margin-top: 10px;
}

input[type="number"] {
  width: 60px;
}

button {
  cursor: pointer;
  padding: 5px 10px;
}
</style>

