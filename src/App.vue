<script setup>
import { onMounted, ref, computed } from 'vue';
import AddForm from './Components/AddForm.vue';

const todos = ref([]);
const query = ref("")
const sortOrder = ref("asc")
async function fetchData() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    let data = await response.json()
    todos.value = data.slice(0, 5)
    console.log(todos.value);
  } catch (error) {
    console.error(error);
  }
}

onMounted(fetchData);
const deleteTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id);
};
const sortedAndFilteredTodos = computed(() => {
  let filtered = todos.value.filter(todo =>
    todo.title.toLowerCase().includes(query.value.toLowerCase())
  );
console.log(filtered);
  return filtered.sort((a, b) => {
    let comparison = 0;
    if (a.id > b.id) {
      comparison = 1;
    } else if (a.id < b.id) {
      comparison = -1;
    }
    return sortOrder.value === 'asc' ? comparison : comparison * -1;
  });
});

const sort = (order) => {
  sortOrder.value = order;
};
</script>

<template>
  <input type="text" placeholder="поиск задачи" v-model="query">
  <button @click="sort('asc')">Сортировать по возрастанию</button>
      <button @click="sort('desc')">Сортировать по убыванию</button>
  <ul>
    <li v-for="(item, index) in sortedAndFilteredTodos" :key="index">
      <p>{{ item.completed }}</p>
      <p>{{ item.id }}</p>
      <p>{{ item.title }}</p>
      <button @click="deleteTodo(item.id)">удалить</button>

    </li>
  </ul>
  <AddForm :todos="todos"></AddForm>
</template>

<style>
*{
  font-family: Arial, Helvetica, sans-serif;
}

ul{
  padding: 20px;
  padding-left: 0px;
  list-style: none;
}

li {
  border: 1px solid;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 10px;
}

button{
  padding: 10px;
  background: none;
  border: 1px solid grey;
}

input{
  padding: 10px;
  margin-right: 10px;
}
</style>