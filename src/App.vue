<template>
  <main>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC"
      crossorigin="anonymous"
    />
    <div>
      <h1>Vue 3 ToDo App</h1>
      <form @submit="addNewTodo">
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

      <section class="btn-section text-nowrap gap-2">
        <span v-if="!edit">
          <button class="btn btn-success" v-on:click="addNewTodo(index)" type="button">Add New ToDo</button>
        </span>
        <span v-else>
          <button class="btn btn-success" @click="updateTodo" type="button">Update ToDo</button>
        </span>

        <button class="btn btn-warning" @click="clearInput" type="button">Clear Input Field</button>
      </section>
      <section class="btn-section text-nowrap gap-2">
        <button @click="removeAll(todo)" class="btn btn-danger">Remove ToDos</button>

        <button @click="markAllasDone" class="btn btn-warning">Mark all as Done</button>
      </section>

      <ul>
        <li v-for="(todo, index) in todos" :key="todo.id" class="todo">
          <h3 :class="{ done: todo.done }" @click="toggleDone(index)">
            {{ todo.content }}
          </h3>
          <section class="btn-section text-nowrap gap-2">
            <button class="btn btn-danger" @click="removeTodo(index)">Remove ToDo</button>
            <button class="btn btn-info" @click="editTodo(index)">Edit ToDo</button>
          </section>
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
    const todos = ref(todoToSave || []);
    let edit = ref(false);
    let selectedIndex = ref(0);
    let done = ref(false);
    let todoToSave = localStorage.getItem("savedTodos");

    function addNewTodo(index) {
      if (edit.value) {
        updateTodo();
      } else if (newTodo.value !== "") {
        todos.value.push({
          id: Date.now(),
          done: false,
          content: newTodo.value
        });
        if (todoToSave) {
          selectedIndex.value = index;
          todoToSave = JSON.stringify(todoToSave);
          todoToSave = JSON.parse(todoToSave);
          if (newTodo.value !== "") {
            todos.value.push({
              id: Date.now(),
              done: false,
              content: newTodo.value
            });
          }
        } else {
          todoToSave = todos.value;
        }
      }
      localStorage.setItem("savedTodos", JSON.stringify(todoToSave));
      newTodo.value = "";
      todos.value = todoToSave;
    }

    function toggleDone(index) {
      todos.value[index].done = !todos.value[index].done;
      done = todos.value[index].done;
      localStorage.setItem("savedTodos", JSON.stringify(todos.value));
    }

    function removeTodo(index) {
      todoToSave.splice(index, 1);
      localStorage.setItem("savedTodos", JSON.stringify(todoToSave));
    }
    function editTodo(index) {
      edit.value = true;
      newTodo.value = todos.value[index].content;
      selectedIndex.value = index;
    }
    function updateTodo() {
      todos.value.splice(selectedIndex.value, 1, { content: newTodo.value });
      clearInput();
    }
    function markAllasDone() {
      todos.value.forEach(element => { 
        element.done = !element.done;
      });
      // todos.value = todoToSave.map(todo => ({ ...todo, done: true }));
      // todoToSave.value = todoToSave.map((x) => {
      //   x.done == true ? (x.done = false) : (x.done = true);
      // });
      // todos.value = todos.value.map((x) => {
      //   x.done == true ? (x.done = false) : (x.done = true);
      // })
      localStorage.setItem("savedTodos", JSON.stringify(todoToSave));
    }

    function removeAll() {
      localStorage.clear();
      todos.value = todoToSave;
    }
    function clearInput() {
      edit.value = false;
      newTodo.value = "";
    }
    return {
      done,
      todoToSave,
      selectedIndex,
      edit,
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
  /* width: 30%; */
  margin: 3% auto;
}
.form-label {
  font-size: 2.5rem;
}
.form-control {
  margin-inline: auto !important;
  width: 250px !important;
}
.input {
  margin-bottom: 3%;
  position: relative;
}
.btn-section {
  display: flex;
  width: 100%;
  justify-content: space-around;
  margin-inline: auto;
  margin-bottom: 2%;
}
.btn-section2 {
  display: flex;
  margin: 0 2% 0 2%;
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
