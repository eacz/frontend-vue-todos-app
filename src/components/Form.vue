<template>
  <div class="">
    <h2>{{ editTodo ? "Editar" : "Agregar" }} To-do</h2>
    <p v-if="error" class="alert alert-danger">{{ error }}</p>
    <form id="form-todo">
      <div class="form group">
        <label for="">Todo</label>
        <input id="todo" class="form-control" type="text" v-model="todo.name" />
      </div>
      <div class="form group">
        <label for="">Descripcion</label
        ><textarea
          class="form-control"
          name=""
          id="description"
          cols="30"
          rows="10"
          v-model="todo.description"
        ></textarea>
      </div>
      <button
        v-if="!editTodo"
        class="btn btn-personalized mt-2"
        @click="agregarTodo"
      >
        Agregar
      </button>
      <button
        v-if="editTodo"
        class="btn btn-personalized mt-2"
        @click="updateTodo"
      >
        Guardar
      </button>
      <button v-if="editTodo" class="btn btn-cancel mt-2" @click="cancelarEdit">
        Cancelar
      </button>
    </form>
  </div>
</template>

<script>
import { local as baseURL } from "../shared/baseURL";
export default {
  data() {
    return {
      todo: {
        name: "",
        description: ""
      },
      error: null
    };
  },
  props: ["todos", "editTodo"],
  methods: {
    agregarTodo: async function(e) {
      e.preventDefault();
      if (!this.todo.name) {
        this.error = "El nombre del to-do esta vacio";
        return;
      }
      if (!this.todo.description) {
        this.error = "La descripcion del to-do esta vacio";
        return;
      }
      try {
        const newTodo = await fetch(`${baseURL}/api/todos`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(this.todo)
        });

        const newTodoJ = await newTodo.json();
        //console.log(newTodoJ);
        this.todos.push(newTodoJ.newTodo);

        this.todo = {
          name: "",
          description: ""
        };
        this.error = null;
      } catch (error) {
        this.error = "Hubo un error con el servidor, intente de nuevo";
      }
    },
    updateTodo: async function(e) {
      e.preventDefault();
      try {
        const todo = await fetch(`${baseURL}/api/todos/${this.todo._id}`, {
          method: "PUT",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            name: this.todo.name,
            description: this.todo.description
          })
        });
        const todoJ = await todo.json();

        this.$emit("todoFinallyUpdated", todoJ.updatedTodo);

        this.todo = {
          name: "",
          description: ""
        };
      } catch (error) {
        this.error = "Hubo un error con el servidor, intente de nuevo";
      }
    },
    cancelarEdit: function() {
      this.$emit("cancelarEdit");
      this.todo = {
        name: "",
        description: ""
      };
    }
  },
  watch: {
    editTodo: function() {
      if (!this.editTodo) {
        return;
      }
      this.todo = this.editTodo;
    }
  }
};
</script>

<style>
@import "../../node_modules/bootstrap/dist/css/bootstrap.min.css";
.btn-personalized {
  background-color: #0028ff;
  color: #f1f3df;
}

.btn-personalized:hover {
  background-color: #24009c;
  color: #f1f3df;
  transition: all 0.5s ease;
}
.btn-cancel {
  background-color: #f1f3df;
  border: 1px solid #24009c;
}
.btn-cancel:hover {
  background-color: #24009c;
  color: #f1f3df;
  transition: all 0.5s ease;
}
</style>
