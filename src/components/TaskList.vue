<template>
  <div class="task-list">
    <p
      v-if="!tasks.length"
      class="empty-list"
    >
      Нет задач
    </p>
    <ul v-else>
      <li
        v-for="task in tasks"
        :key="task.id"
        :class="{ completed: task.completed }"
      >
        <div
          class="task-content"
          v-if="!task.editing"
        >
          <div class="task-info">
            <input
              type="checkbox"
              :checked="task.completed"
              @change="toggleTask(task.id)"
            />
            <span class="task-text">{{ task.text }}</span>
          </div>
          <div class="task-actions">
            <button
              class="edit-btn"
              @click="startEditing(task)"
            >
              Редактировать
            </button>
            <button
              class="delete-btn"
              @click="deleteTask(task.id)"
            >
              Удалить
            </button>
          </div>
        </div>
        <div
          class="task-edit"
          v-else
        >
          <input
            type="text"
            v-model="editText"
            @keyup.enter="finishEditing(task)"
          />
          <div class="edit-actions">
            <button
              class="save-btn"
              @click="finishEditing(task)"
            >
              Сохранить
            </button>
            <button
              class="cancel-btn"
              @click="cancelEditing(task)"
            >
              Отменить
            </button>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps({
  tasks: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["toggle-task", "edit-task", "delete-task"]);

const editText = ref("");

const toggleTask = (id) => {
  emit("toggle-task", id);
};

const startEditing = (task) => {
  props.tasks.forEach((t) => (t.editing = false));
  task.editing = true;
  editText.value = task.text;
};

const finishEditing = (task) => {
  const text = editText.value.trim();
  if (text) {
    emit("edit-task", task.id, text);
  }
  task.editing = false;
};

const cancelEditing = (task) => {
  task.editing = false;
};

const deleteTask = (id) => {
  emit("delete-task", id);
};
</script>

<style scoped>
.task-list {
  margin-top: 20px;
}

.empty-list {
  text-align: center;
  color: #999;
  font-style: italic;
}

ul {
  list-style-type: none;
}

li {
  margin-bottom: 10px;
  padding: 12px;
  border-radius: 4px;
  background-color: #f9f9f9;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  transition: background-color 0.2s;
}

li:hover {
  background-color: #f5f5f5;
}

.task-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-info {
  display: flex;
  align-items: center;
  gap: 10px;
  flex: 1;
}

input[type="checkbox"] {
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.task-text {
  word-break: break-word;
}

.completed .task-text {
  text-decoration: line-through;
  color: #888;
}

.task-actions {
  display: flex;
  gap: 5px;
}

.task-edit {
  display: flex;
  gap: 10px;
  flex-direction: column;
}

.task-edit input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.edit-actions {
  display: flex;
  gap: 5px;
}

button {
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.2s;
}

.edit-btn {
  background-color: #4a90e2;
  color: white;
}

.edit-btn:hover {
  background-color: #3a80d2;
}

.delete-btn {
  background-color: #e74c3c;
  color: white;
}

.delete-btn:hover {
  background-color: #d73c2c;
}

.save-btn {
  background-color: #42b983;
  color: white;
}

.save-btn:hover {
  background-color: #3aa876;
}

.cancel-btn {
  background-color: #7f8c8d;
  color: white;
}

.cancel-btn:hover {
  background-color: #6f7c7d;
}

@media (max-width: 580px) {
  .task-content {
    flex-direction: column;
    align-items: flex-start;
  }

  .task-actions {
    margin-top: 10px;
    width: 100%;
    justify-content: space-between;
  }

  .task-edit {
    width: 100%;
  }

  .edit-actions {
    justify-content: space-between;
  }

  button {
    flex: 1;
  }
}
</style>
