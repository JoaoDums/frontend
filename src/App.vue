<script setup>

import { ref, onMounted } from "vue";
import axios from "axios";

import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";

const tasks = ref([]);

const newTask = ref("");
const newDescription = ref("");

async function getTasks() {

  const response = await axios.get(
    "http://localhost:3000/tasks"
  );

  tasks.value = response.data;
}

async function createTask() {

  if (!newTask.value) return;

  await axios.post(
    "http://localhost:3000/tasks",
    {
      title: newTask.value,
      description: newDescription.value
    }
  );

  newTask.value = "";
  newDescription.value = "";

  getTasks();
}

async function deleteTask(taskId) {

  await axios.delete(
    `http://localhost:3000/tasks/${taskId}`
  );

  getTasks();
}

async function toggleTask(task) {

  await axios.put(
    `http://localhost:3000/tasks/${task.id}`,
    {
      done: !task.done
    }
  );

  getTasks();
}

onMounted(() => {
  getTasks();
});

</script>


<template>
  <div class="app">
    <div class="container">
      <h1> Lista de Tarefas</h1>

      <TaskForm
        :newTask="newTask"
        :newDescription="newDescription"
        @update:newTask="newTask = $event"
        @update:newDescription="newDescription = $event"
        @createTask="createTask"
      />

      <TaskList
        :tasks="tasks"
        @deleteTask="deleteTask"
        @toggleTask="toggleTask"
      />
    </div>
  </div>
</template>


<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  padding-top: 60px;
  background: linear-gradient(135deg, #003cff, #7baee2);
  font-family: Arial, sans-serif;
}

.container {
  width: 100%;
  max-width: 520px;
  background: rgb(88, 166, 202);
  border-radius: 20px;
  padding: 20px;
  box-shadow: 0 12px 35px rgba(0,0,0,0.1);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  font-size: 22px;
}
</style>