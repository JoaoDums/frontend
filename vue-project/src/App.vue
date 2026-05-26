
<script setup>

import { ref, onMounted } from "vue";
import axios from "axios";

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
  const updatedDone = !task.done;
  await axios.put(
    `http://localhost:3000/tasks/${task.id}`,
    {
      done: updatedDone
    }
  );

  getTasks();
}

onMounted(() => {
  getTasks();
});

</script>



<template>
  <div>

    <h1>Todo App</h1>

    <input
      v-model="newTask"
      type="text"
      placeholder="Digite uma tarefa"
    />

<input
  v-model="newDescription"
  type="text"
  placeholder="Digite uma descrição"
/>

    <button @click="createTask">
      Adicionar
    </button>

    <ul>

      <li
        v-for="task in tasks"
        :key="task.id"
      >

      <input
          type="checkbox"
          :checked="task.done"
          @change="toggleTask(task)"
        />

        <span
          :style="{
            textDecoration: task.done
              ? 'line-through'
              : 'none'
          }"
        >
          {{ task.title }}
          <p>
          {{ task.description }}        
        </p>
        </span>

        <button @click="deleteTask(task.id)">
          Excluir
        </button>

      </li>

    </ul>

  </div>
</template>

<style scoped>

div {
  padding: 20px;
}

input {
  padding: 8px;
  margin-right: 10px;
}

button {
  margin-left: 10px;
  padding: 6px;
}
li {
  margin-top: 10px;
}

</style>