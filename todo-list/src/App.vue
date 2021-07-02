<template>
<main>
  <link
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css"
    rel="stylesheet"
    integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC"
    crossorigin="anonymous"/>
  <div>
    <h1>Vue 3 ToDo App</h1>
    <form @submit.prevent="addNewTodo(index)">
      <div class="mb-3 centralized">
        <label for="formGroupExampleInput" class="form-label"> ToDo List </label>

        <input
          type="text"
          class="form-control input"
          id="formGroupExampleInput"
          placeholder="New ToDo"
          v-model="newTodo"
        />
      </div>
    </form>

    <section class="btn-section">
      <span v-if="edit !== true">
        <button class="btn btn-success" @click.prevent="addNewTodo" type="button">Add New ToDo</button>
      </span>
      <span v-else>
        <button class="btn btn-success" @click.prevent="updateTodo(index)" type="button">Update ToDo</button>
      </span>

      <button class="btn btn-warning" @click.prevent="clearInput" type="button">Clear Input Field</button>
    </section>
    <section class="btn-section">
      <button @click="removeAll" class="btn btn-danger">Remove ToDos</button>

      <button @click="markAllasDone" class="btn btn-warning">Mark all as Done</button>
    </section>

    <ul>
      <li v-for="(todo, index) in todos" :key="todo.id" class="todo">
        <h3 :class="{ done: todo.done }" @click="toggleDone(todo)">
          {{ todo.content }}
        </h3>

        <button @click="removeTodo(index)">Remove ToDo</button>
        <button @click="editTodo(index)">Edit ToDo</button>
      </li>
    </ul>
  </div>
  </main>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const newTodo = ref("");
    const todos = ref([]);
    let edit = ref(false);

    function addNewTodo() {
      if (newTodo.value !== "") {
        todos.value.push({
          id: Date.now(),
          done: false,
          content: newTodo.value
        });
        console.log(edit.value)
      }
      newTodo.value = "";
    }
    function toggleDone(todo) {
      todo.done = !todo.done;
    }
    function removeTodo(index) {
      todos.value.splice(index, 1);
    }
    function editTodo(index) {
      edit.value = true;
      console.log(edit.value);
      newTodo.value = todos.value[index].content;
    }
    function updateTodo() {}
    function markAllasDone() {
      todos.value.forEach((todo) => (todo.done = true));
    }
    function removeAll() {
      todos.value = [];
    }
    function clearInput() {
      newTodo.value = "";
    }
    return {
      clearInput,
      removeAll,
      markAllasDone,
      removeTodo,
      editTodo,
      updateTodo,
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
.title {
  display: block;
  font-size: 2.5rem;
}
ul {
  padding: 0 !important;
}
.centralized {
  width: 30%;
  margin: 3% auto;
}
.form-label {
  font-size: 2.5rem;
}
.input {
  margin-bottom: 3%;
  position: relative;
}
.btn-section {
  display: flex;
  width: 30%;
  justify-content: space-between;
  margin-inline: auto;
  margin-bottom: 2%;
}

.todo {
  cursor: pointer;
}
.done {
  text-decoration: line-through;
}
</style>
