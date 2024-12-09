<template>
  <div class="container">
    <h1>Task Management App</h1>
    <button @click="toggleForm">
      {{ showForm ? "Hide Add Task Form" : "Show Add Task Form" }}
    </button>

    <form v-if="showForm" @submit.prevent="addTask">
      <input v-model="newTask" placeholder="Enter task name" />
      <button type="submit">Add Task</button>
    </form>

    <p v-if="tasks.length === 0">No tasks available</p>
    <ul>
      <li
        v-for="(task, index) in tasks"
        :key="index"
        :class="{ completed: task.completed }"
        class="task-item"
      >
        <div>
          {{ task.name }}
        </div>
        <div>
          <button @click="toggleComplete(index)">
            {{ task.completed ? "Undo" : "Complete" }}
          </button>
          <button @click="deleteTask(index)">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { reactive, ref } from "vue";

export default {
  setup() {
    const tasks = reactive(JSON.parse(localStorage.getItem("tasks")) || []);
    const newTask = ref("");
    const showForm = ref(false);

    const saveTasks = () =>
      localStorage.setItem("tasks", JSON.stringify(tasks));

    const toggleForm = () => (showForm.value = !showForm.value);

    const addTask = () => {
      if (newTask.value.trim().length < 3) {
        alert("Task name must have at least 3 characters.");
        return;
      }
      tasks.push({ name: newTask.value, completed: false });
      newTask.value = "";
      saveTasks();
    };

    const toggleComplete = (index) => {
      tasks[index].completed = !tasks[index].completed;
      saveTasks();
    };

    const deleteTask = (index) => {
      tasks.splice(index, 1);
      saveTasks();
    };

    return {
      tasks,
      newTask,
      addTask,
      toggleComplete,
      deleteTask,
      showForm,
      toggleForm,
    };
  },
};
</script>

<style>
.task-item button {
  margin: 0 10px;
  text-transform: uppercase;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 10px;
  border: 2px solid #606c76;
  border-radius: 15px;
}
.completed {
  background-color: rgb(12, 116, 12);
  color: #fff;
  border: 2px solid #9b4dca;
  border-radius: 15px;
}
input {
  color: #606c76;
}
</style>
