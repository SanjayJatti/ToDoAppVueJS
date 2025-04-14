<script setup>
import { ref } from "vue";
const props = defineProps(["todos"]);
const emit = defineEmits(["toggle", "delete", "update"]);

const editId = ref(null);
const editTitle = ref("");

const handleToggleTodo = (id) => {
  emit("toggle", id);
};
const deleteTodo = (id) => {
  emit("delete", id);
};
const handleEdit = (todo) => {
  editId.value = todo.id;
  editTitle.value = todo.title;

};
const handleSaveEdit = (id) => {
  emit("update", { id, title: editTitle.value });
  editId.value = null;
  editTitle.value = "";
};
const handleCancelEdit = () => {
  editId.value = null;
};
</script>
<template>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      <div v-if="editId !== todo.id">
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
        <button @click="handleEdit(todo)">Edit</button>
      </div>
      <div v-else>
        <input type="text" v-model="editTitle" @keyup="(event)=> {if(event.key === 'Enter') handleSaveEdit(todo.id) }"/>
        <button @click="handleSaveEdit(todo.id)">Save</button>
        <button @click="handleCancelEdit">Cancel</button>
      </div>
    </li>
  </ul>
</template>
<style scoped>
.todo {
  margin-right: 0.5rem;
}
</style>
