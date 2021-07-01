<template>
  <div>
    <h1>Vue 3 To Do App</h1>
    <form @submit.prevent="addNewTodo">
      <label>New To Do</label>
      <input v-model="newTodo" name="newTodo" />
      <button>Add New To Do</button>
    </form>
    <button @click="removeAll">Remove All To Dos</button>
    <button @click="markAllasDone">Mark all as Done</button>
    <ul>
      <li v-for="(todo, index) in todos" :key="todo.id" class="todo">
        <h3 :class="{done: todo.done }" @click="toggleDone(todo)">{{ todo.content }}</h3>
        <button @click="removeTodo(index)">Remove To Do</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const newTodo = ref("");
    const todos = ref([]);

    function addNewTodo() {
      todos.value.push({
        id: Date.now(),
        done: false,
        content: newTodo.value
      });
      newTodo.value = '';
    }
    function toggleDone(todo){
      todo.done = !todo.done
    }
    function removeTodo(index){
      todos.value.splice(index, 1)
    }
    function markAllasDone(){
      todos.value.forEach((todo) => todo.done = true)
    }
    function removeAll(){
      todos.value = []
    }
    return {
      removeAll,
      markAllasDone,
      removeTodo,
      toggleDone,
      todos,
      newTodo,
      addNewTodo
    };
  }
};
</script>

<style>
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.todo{
  cursor:pointer;
}
.done {
  text-decoration:line-through;
}
</style>
