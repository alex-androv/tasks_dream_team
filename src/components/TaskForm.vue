<template>
  <div class="task-form">
    <input
      type="text"
      v-model="taskText"
      placeholder="Введите новую задачу"
      @keyup.enter="submitTask"
    />
    <button
      @click="submitTask"
      :disabled="!taskText.trim()"
    >
      Добавить
    </button>
  </div>
</template>

<script setup>
import { ref } from "vue";

const taskText = ref("");

const emit = defineEmits(["add-task"]);

const submitTask = () => {
  const text = taskText.value.trim();
  if (text) {
    emit("add-task", text);
    taskText.value = "";
  }
};
</script>

<style scoped>
.task-form {
  display: flex;
  margin-bottom: 20px;
  gap: 10px;
}

input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

button {
  padding: 10px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3aa876;
}

button:disabled {
  background-color: #a8d5c5;
  cursor: not-allowed;
}

@media (max-width: 480px) {
  .task-form {
    flex-direction: column;
  }

  button {
    width: 100%;
  }
}
</style>
