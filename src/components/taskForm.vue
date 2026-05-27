<script setup>

defineProps({
  newTask: String,
  newDescription: String
});

defineEmits([
  "update:newTask",
  "update:newDescription",
  "createTask"
]);

</script>

<template>
  <div class="form">
    <input
      :value="newTask"
      @input="$emit('update:newTask', $event.target.value)"
      type="text"
      placeholder="Nova tarefa..."
    />

    <input
      :value="newDescription"
      @input="$emit('update:newDescription', $event.target.value)"
      type="text"
      placeholder="Descrição"
    />

    <button @click="$emit('createTask')">
      Adicionar
    </button>
  </div>
</template>

<style scoped>

.form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #ddd;
  outline: none;
  transition: 0.2s;
}

input:focus {
  border-color: #6366f1;
  box-shadow: 0 0 0 3px rgba(99,102,241,0.2);
}

button {
  padding: 10px;
  border: none;
  border-radius: 8px;
  background: #6366f1;
  color: white;
  cursor: pointer;
  transition: 0.2s;
}

button:hover {
  background: #4f46e5;
}


</style>