<template>
  <div class="todo-app">
    <h1>Список задач</h1>
    <div class="container">
      <TaskForm @add-task="addTask" />
      <TaskFilter
        :activeFilter="activeFilter"
        @change-filter="changeFilter"
      />
      <TaskList
        :tasks="filteredTasks"
        @toggle-task="toggleTask"
        @edit-task="editTask"
        @delete-task="deleteTask"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from "vue";
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";
import TaskFilter from "./components/TaskFilter.vue";

const tasks = ref([]);
const activeFilter = ref("all");

onMounted(() => {
  const savedTasks = localStorage.getItem("tasks");
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
  }
});

watch(
  tasks,
  (newTasks) => {
    localStorage.setItem("tasks", JSON.stringify(newTasks));
  },
  { deep: true }
);

const filteredTasks = computed(() => {
  switch (activeFilter.value) {
    case "completed":
      return tasks.value.filter((task) => task.completed);
    case "active":
      return tasks.value.filter((task) => !task.completed);
    default:
      return tasks.value;
  }
});

const addTask = (text) => {
  tasks.value.push({
    id: Date.now(),
    text,
    completed: false,
    editing: false,
  });
};

const toggleTask = (id) => {
  const task = tasks.value.find((task) => task.id === id);
  if (task) {
    task.completed = !task.completed;
  }
};

const editTask = (id, text) => {
  const task = tasks.value.find((task) => task.id === id);
  if (task) {
    task.text = text;
  }
};

const deleteTask = (id) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
};

const changeFilter = (filter) => {
  activeFilter.value = filter;
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Arial", sans-serif;
  background-color: #f5f5f5;
  color: #333;
}

.todo-app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #2c3e50;
}

.container {
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
}

@media (max-width: 650px) {
  .todo-app {
    padding: 10px;
  }

  .container {
    padding: 15px;
  }
}

@media (max-width: 480px) {
  h1 {
    font-size: 24px;
  }

  .container {
    padding: 10px;
  }
}
</style>
