<script setup>
import TodoList from "./components/TodoList.vue";
import AddTodo from "./components/AddTodo.vue";
import { ref, onMounted, computed, watch } from "vue";
import axios from "axios";

const todos = ref([]);
const filter = ref("all");

watch(
  todos,
  (newTodos) => {
    localStorage.setItem("todos", JSON.stringify(newTodos));
  },
  { deep: true }
);

const fetchTodos = async () => {
  const storedTodos = localStorage.getItem("todos");
  if (storedTodos) {
    todos.value = JSON.parse(storedTodos);
  } else {
    try {
      const res = await axios.get(
        "https://jsonplaceholder.typicode.com/todos?_limit=10"
      );
      todos.value = res.data;
    } catch (err) {
      console.log(err);
    }
  }
};
const addTodo = (todo) => {
  todos.value.unshift(todo);
  filter.value = "all";
};
const toggleTodo = (id) => {
  const todo = todos.value.find((item) => item.id === id);
  if (todo) todo.completed = !todo.completed;
};

const handleDeleteTodo = (id) => {
  todos.value = todos.value.filter((item) => item.id !== id);
};
const handleEditTodo = ({ id, title }) => {
  const targetTodo = todos.value.find((item) => item.id === id);
  if (targetTodo) targetTodo.title = title;
};
onMounted(fetchTodos);

const filteredTodos = computed(() => {
  if (filter.value === "completed")
    return todos.value.filter((item) => item.completed);
  if (filter.value === "incomplete")
    return todos.value.filter((item) => !item.completed);
  return todos.value;
});

const showCompletedOnly = () => {
  return (filter.value = "completed");
};
const showIncompletedOnly = () => {
  return (filter.value = "incomplete");
};

const showAll = () => {
  return (filter.value = "all");
};
</script>

<template>
  <h3>To Do List</h3>
  <AddTodo @add="addTodo" />
  <div class="m-t-16">
    <button @click="showAll">All</button>
    <button @click="showCompletedOnly">Completed</button>
    <button @click="showIncompletedOnly">Incomplete</button>
  </div>
  <TodoList
    :todos="filteredTodos"
    @toggle="toggleTodo"
    @delete="handleDeleteTodo"
    @update="handleEditTodo"
  />
</template>

<style scoped>
.m-t-16 {
  margin-top: 16px;
}
</style>
