<script setup>
const props = defineProps(["todos"]);
const emit = defineEmits(["toggle", "delete"]);

const handleToggleTodo = (id) => {
  emit("toggle", id);
};
const deleteTodo = (id) => {
  emit("delete", id);
};
</script>
<template>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="handleToggleTodo(todo.id)"
      />
      <span
        :style="{ textDecoration: todo.completed ? 'line-through' : 'none' }"
        class="todo"
      >
        {{ todo.title }}</span
      >
      <button @click="deleteTodo(todo.id)">X</button>
    </li>
  </ul>
</template>
<style scoped>
.todo {
  margin-right: 0.5rem;
}
</style>
