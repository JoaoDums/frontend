<script setup>

defineProps({
  task: Object
});

defineEmits([
  "deleteTask",
  "toggleTask"
]);

</script>


<template>
  <li class="task">
    <div class="left">
      <input
        type="checkbox"
        :checked="task.done"
        @change="$emit('toggleTask', task)"
      />

      <div class="text">
        <span :class="{ done: task.done }">
          {{ task.title }}
        </span>

        <small>{{ task.description }}</small>
      </div>
    </div>

    <button class="delete" @click="$emit('deleteTask', task.id)">
      ✖
    </button>
  </li>
</template>



<style scoped>
.task {
  display: flex;
  justify-content: space-between;
  align-items: center;

  padding: 12px;
  margin-bottom: 10px;

  border-radius: 10px;
  background: #f9fafb;
  border: 1px solid #eee;

  transition: 0.2s;
}

.task:hover {
  transform: translateY(-2px);
  background: #f3f4f6;
}

.left {
  display: flex;
  align-items: center;
  gap: 10px;
}

.text {
  display: flex;
  flex-direction: column;
}

.text small {
  color: #888;
  font-size: 12px;
}

.done {
  text-decoration: line-through;
  color: #999;
}

.delete {
  background: transparent;
  border: none;
  cursor: pointer;
  color: #888;
  font-size: 16px;
}

.delete:hover {
  color: red;
}


</style>