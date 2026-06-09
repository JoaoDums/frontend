<script setup>

import TaskItem from "./taskItem.vue";

defineProps({
  tasks: Array
});

defineEmits([
  "deleteTask",
  "toggleTask"
]);

</script>

<template>
  <ul class="list">
    <TaskItem
      v-for="task in tasks"
      :key="task.id"
      :task="task"
      @deleteTask="$emit('deleteTask', $event)"
      @toggleTask="$emit('toggleTask', $event)"
    />

    <p v-if="tasks.length === 0" class="empty">
      Nenhuma tarefa cadastrada.
    </p>
  </ul>
</template>



<style scoped>

    .list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.empty {
  text-align: center;
  color: #000000;
  margin-top: 20px;
}
</style>