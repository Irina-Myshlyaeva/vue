<script setup>
import { onMounted, ref } from 'vue';

const inputValue = ref("")  
const props = defineProps({
    todos:{
        type:Array
    }
})
const addTodo = () => {
  const newTodoItem = {
    id: 1,
    title: inputValue.value,
    completed: false,
  };
  console.log(props.todos);
  props.todos.push(newTodoItem);
  inputValue.value = "";

};
</script>
<template>
    <form @submit.prevent="addTodo">
        <input type="text" placeholder="описание задачи" v-model = "inputValue">
        <button type="submit">добавить</button>
    </form>
</template>