<template>
  <div id="app">
    <Header />
    <main class="container">
      <Todos
        class="todos"
        :todos="todos"
        v-on:updateTodo="updateTodo($event)"
        v-on:editTodo="setEditTodo($event)"
      />
      <Form
        :todos="todos"
        :editTodo="todoEdit"
        v-on:cancelarEdit="cancelarEdit"
        v-on:todoFinallyUpdated="todoFinallyUpdated($event)"
      />
    </main>
  </div>
</template>

<script>
import Todos from "./components/Todos";
import Form from "./components/Form";
import Header from "./components/Layout/Header";
export default {
  name: "app",
  components: { Todos, Form, Header },
  data() {
    return {
      todos: [],
      todoEdit: null
    };
  },
  beforeCreate: async function() {
    const data = await fetch("http://localhost:4000/api/todos");
    const todos = await data.json();

    this.todos = todos;
    console.log(todos);
  },
  methods: {
    updateTodo: function(updatedTodos) {
      this.todos = updatedTodos;
    },
    setEditTodo: function(todoID) {
      this.todoEdit = this.todos.find(todo => todo._id === todoID);
    },
    cancelarEdit: function() {
      this.todoEdit = null;
    },
    todoFinallyUpdated: function(todoUpdated) {
      //console.log(todoUpdated);
      this.todos.filter(todo =>
        todo._id === todoUpdated._id ? todoUpdated : todo
      );
    }
  }
};
</script>

<style>
@import url(https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.min.css);
body {
  background-color: #f1f3df;
}
.container {
  display: flex;
  flex-direction: column;
}

@media (min-width: 768px) {
  .container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    column-gap: 2rem;
  }
  .todos {
    grid-column: 1/3;
  }
}
</style>
