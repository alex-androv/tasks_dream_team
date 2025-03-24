<template>
  <div class="task-filter">
    <button
      v-for="filter in filters"
      :key="filter.value"
      :class="{ active: activeFilter === filter.value }"
      @click="changeFilter(filter.value)"
    >
      {{ filter.label }}
    </button>
  </div>
</template>

<script setup>
const props = defineProps({
  activeFilter: {
    type: String,
    required: true,
  },
});

const emit = defineEmits(["change-filter"]);

const filters = [
  { value: "all", label: "Все задачи" },
  { value: "active", label: "Невыполненные" },
  { value: "completed", label: "Выполненные" },
];

const changeFilter = (filter) => {
  emit("change-filter", filter);
};
</script>

<style scoped>
.task-filter {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 15px;
}

button {
  padding: 5px 10px;
  background-color: #f1f1f1;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s;
}

button:hover {
  background-color: #e5e5e5;
}

button.active {
  background-color: #42b983;
  color: white;
  border-color: #42b983;
}

@media (max-width: 480px) {
  .task-filter {
    flex-direction: column;
  }

  button {
    width: 100%;
    padding: 8px;
  }
}
</style>
