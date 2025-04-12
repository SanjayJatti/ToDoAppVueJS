<script setup>
import {ref} from 'vue';
const newTodo = ref('');
const emit = defineEmits(['add']);


const handleAddTodo=()=>{
    if(!newTodo.value.trim()) return

    const newItem = {
        id: Date.now(),
        title: newTodo.value,
        completed : false
    }
    emit('add', newItem);
    newTodo.value = ''
}

</script>
<template>
    <form @submit.prevent="handleAddTodo">
        <input type="text" placeholder="Enter Value..." v-model="newTodo"/>
        <button type="submit">Add</button>
    </form>

</template>