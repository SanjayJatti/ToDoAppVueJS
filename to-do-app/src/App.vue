<script setup>
import TodoList from "./components/TodoList.vue";
import AddTodo from "./components/AddTodo.vue";
import { ref, onMounted } from "vue";
import axios from "axios";

const todos = ref([]);

const fetchTodos = async () => {
  try {
    const res = await axios.get(
      "https://jsonplaceholder.typicode.com/todos?_limit=10"
    );
    todos.value = res.data;
  } catch (err) {
    console.log(err);
  }
};
const addTodo = (todo) => {
  todos.value.unshift(todo);
};
const toggleTodo = (id) => {
  const todo = todos.value.find((item) => item.id === id);
  if (todo) todo.completed = !todo.completed;
};

const handleDeleteTodo = (id) => {
  todos.value = todos.value.filter((item) => item.id !== id);
};
const handleEditTodo= ({id, title})=>{
  const targetTodo = todos.value.find(item => item.id === id);
  if(targetTodo) targetTodo.title = title;
}
onMounted(fetchTodos);
</script>

<template>
  <h3>To Do List</h3>
  <AddTodo @add="addTodo" />
  <TodoList :todos="todos" @toggle="toggleTodo" @delete="handleDeleteTodo" @update="handleEditTodo" />
</template>

<style scoped></style>
