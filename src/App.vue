<template>

<div class="container-title">
  <h1>to do</h1>
</div>

<div class="container-form">
  <form @submit.prevent="addTodo()">
    <div id="input">
    <q-input 
    rounded outlined 
    v-model="newTodo" 
    name="newTodo" 
    label="Type here" 
    autocomplete="off"
    />
    </div>
    <div>
    <q-btn @click="addTodo()" color="purple" label="Add To Do" />
    </div>
  </form>
</div>

<div>
  <h2>list</h2>
  <ul>
    <li v-for="(todo, index) in todos" :key="index">
      <span :class="{ done: todo.done }" @click="doneTodo(todo)">
        {{todo.content}}
      </span> 

      <q-btn push @click="removeTodo(index)" label="Remove" color="red" />
    </li>
    
  </ul>
  <h4 v-if="todos.length === 0">-- Insert item --</h4>
</div>

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
  height: 100%;
}

input {
    margin: 100px;
}

html,body {
  height: 100%;
  background:  #f5f5f5
}

form {
    max-width: 100%;
    width: 500px;
    color: white;
    display: inline-block;
    margin: 20px;
}

ul {
  text-align: center;
}

li {
  width: 1000px;
  font-family: 'Comfortaa', cursive;
  display: inline-block;
  font-size: 50px;
  background-color: rgb(175, 135, 214);
  padding: 20px;
  margin: 20px;
  border-radius: 25px;
}

h1 {
  font-family: 'Readex Pro', sans-serif;
  font-size: 100rem;
  color: rgb(175, 135, 214);
  margin: 10px;
}

h2 {
  font-family: 'Readex Pro', sans-serif;
  margin: 5px;
}

@font-face {
  font-family: 'Comfortaa', cursive;
  src: url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@611;700&display=swap');
  font-family: 'Readex Pro', sans-serif;
  src: url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@611;700&family=Readex+Pro:wght@700&display=swap');
}

</style>

