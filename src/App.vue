<script setup>

import { ref, onMounted } from "vue";
import axios from "axios";
import TaskForm from "./components/taskForm.vue";
import TaskList from "./components/taskList.vue";

const API_URL = "https://backend-ht8e.onrender.com";
const tasks = ref([]);
const email = ref("");
const password = ref("");

const token = ref(
  localStorage.getItem("token") || ""
);

const newTask = ref("");
const newDescription = ref("");


async function login() {

  try {

    const response = await axios.post(
      `${API_URL}/login`,
      {
        email: email.value,
        password: password.value
      }
    );

    token.value = response.data.token;

    localStorage.setItem(
      "token",
      token.value
    );

    getTasks();

  } catch (error) {

    alert("Email ou senha inválidos");

  }

}

async function deleteTask(taskId) {

  await axios.delete(
    `${API_URL}/tasks/${taskId}`,
    {
      headers: {
        Authorization: `Bearer ${token.value}`
      }
    }
  );

  getTasks();
}

function logout() {

  localStorage.removeItem(
    "token"
  );

  token.value = "";

  tasks.value = [];

}

async function getTasks() {

  const response = await axios.get(
    `${API_URL}/tasks`,
    {
      headers: {
        Authorization: `Bearer ${token.value}`
      }
    }
  );

  tasks.value = response.data;
}

async function createTask() {

  if (!newTask.value) return;

  await axios.post(
    `${API_URL}/tasks`,
    {
      title: newTask.value,
      description: newDescription.value
    },
    {
      headers: {
        Authorization: `Bearer ${token.value}`
      }
    }
  );

  newTask.value = "";
  newDescription.value = "";

  getTasks();
}


async function toggleTask(task) {

  await axios.put(
    `${API_URL}/tasks/${task.id}`,
    {
      done: !task.done
    }
  );

  getTasks();
}

onMounted(() => {

  if (token.value) {
    getTasks();
  }

});

</script>


<template>

  <div v-if="!token" class="app">

    <div class="container">

      <h1>Login</h1>

      <input
        v-model="email"
        placeholder="Email"
      />

      <input
        v-model="password"
        type="password"
        placeholder="Senha"
      />

      <button @click="login">
        Entrar
      </button>

    </div>

  </div>

  <div v-else class="app">

    <button @click="logout">
  sair
    </button>

<div class="container">

      <h1>Lista de Tarefas</h1>

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