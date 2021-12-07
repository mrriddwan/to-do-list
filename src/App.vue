<template>
  <h1 c>To Do App</h1>
  <form @submit.prevent="addTodo()">
    <label class="main-title">New To Do </label>
    <input v-model="newTodo" name="newTodo" autocomplete="off" />
    <q-btn @click="addTodo()" color="primary" label="Add To Do" />
  </form>
  <h2>To Do List</h2>
  <ul>
    <li v-for="(todo, index) in todos" :key="index">
      <span :class="{ done: todo.done }" @click="doneTodo(todo)">{{
        todo.content
      }}</span>
      <button @click="removeTodo(index)">Remove</button>
    </li>
  </ul>
  <h4 v-if="todos.length === 0">Empty list.</h4>
</template>

<script>
import { ref } from 'vue';
export default {
  name: 'App',
  setup() {
    const newTodo = ref('');
    const defaultData = [
      {
        done: false,
        content: 'Write a blog post',
      },
    ];
    const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
    const todos = ref(todosData);
    function addTodo() {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value,
        });
        newTodo.value = '';
      }
      saveData();
    }
    function doneTodo(todo) {
      todo.done = !todo.done;
      saveData();
    }
    function removeTodo(index) {
      todos.value.splice(index, 1);
      saveData();
    }
    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem('todos', storageData);
    }
    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  background-color: #708CB0;
  height: 100%;
}

input {
    margin: 10px;
}

.main-title {
    font: text-weight-bolder;
}
</style>
