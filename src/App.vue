<template>
  <div id="app">
    <Header v-on:deleteAllTodos="deleteAllTodos" />
    <main class="container-m">
      <div class="header-body">
        <h1>To-dos</h1>
        <Filtro :filtrado="filtrado" v-on:changeFilter="changeFilter($event)" />
      </div>
      <Spinner v-if="!todos" />
      <Todos
        :todos="todosFiltered"
        v-on:updateTodo="updateTodo($event)"
        v-on:editTodo="setEditTodo($event)"
      />

      <Form
        ref="form-todo"
        :todos="todos"
        :editTodo="todoEdit"
        v-on:cancelarEdit="cancelarEdit"
        v-on:todoFinallyUpdated="todoFinallyUpdated($event)"
      />
      <button class="floatButton" @click="focusForm"><p>+</p></button>
    </main>
  </div>
</template>

<script>
import Todos from "./components/Todos";
import Form from "./components/Form";
import Header from "./components/Layout/Header";
import Spinner from "./components/Layout/Spinner";
import Filtro from "./components/Filtro";
import baseURL from "./shared/baseURL";
export default {
  name: "app",
  components: { Todos, Form, Header, Filtro, Spinner },
  data() {
    return {
      todos: null,
      todoEdit: null,
      filtrado: "default"
    };
  },
  beforeCreate: async function() {
    const data = await fetch(`${baseURL}/api/todos`);
    const todos = await data.json();

    this.todos = todos;
    //console.log(todos);
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
      this.todoEdit = null;
    },
    changeFilter: function(filter) {
      if (filter) {
        this.filtrado = filter;
      }
    },
    focusForm: function() {
      this.$refs.form - todo.focus();
    },
    deleteAllTodos: async function() {
      try {
        await fetch(`${baseURL}/api/todos`, {
          method: "DELETE",
          headers: { "Content-Type": "application/json" }
        });
        this.todos = [];
      } catch (error) {
        console.log(error);
      }
    }
  },
  computed: {
    todosFiltered: function() {
      switch (this.filtrado) {
        case "terminado":
          return this.todos.filter(todo => todo.done);
        case "pendiente":
          return this.todos.filter(todo => !todo.done);
        case "fecha-asc":
          function orderByDate(a, b) {
            if (Date.parse(a.created) < Date.parse(b.created)) return 1;
            else return -1;
          }
          return this.todos.sort(orderByDate);
        case "fecha-des":
          function orderByDate(a, b) {
            if (Date.parse(a.created) > Date.parse(b.created)) return 1;
            else return -1;
          }
          return this.todos.sort(orderByDate);
        default:
          return this.todos;
      }
    }
  }
};
</script>

<style>
@import url(https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.min.css);

html {
  scroll-behavior: smooth;
}
body,
#app {
  background-color: #f1f3df;
}
.container-m {
  max-width: 1200px;
  width: 95%;
  margin: 0 auto;
}

@media (min-width: 768px) {
  .container-m {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    column-gap: 2rem;
  }
  .todos {
    grid-column: 1/3;
  }
  .spinner {
    grid-column: 1/3;
  }
  .header-body {
    grid-column: 1/3;
  }
  .floatButton {
    display: none;
  }
}

@media (min-width: 1024px) {
  .header-body {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .header-body h1 {
    min-width: 200px;
  }
}

.floatButton {
  border: none;
  position: fixed;
  background-color: #10eaf0;
  border-radius: 50px;
  width: 60px;
  height: 60px;
  bottom: 40px;
  right: 40px;
  text-align: center;
  font-size: 2rem;
  box-shadow: 2px 2px 3px #999;
  color: #f1f3df;
}

.floatButton:hover,
.floatButton:active {
  color: #f1f3df;
  text-decoration: none;
  background-color: #0028ff;
  transition: all 0.5s ease-in-out;
}
</style>
