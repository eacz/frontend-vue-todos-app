<template>
  <div class="todo">
    <div class="todo-header">
      <div class="title">
        <h4 class="todo-title">
          <p>{{ todo.name }}</p>
          <span
            class="badge "
            v-bind:class="{
              'badge-success': todo.done,
              'badge-danger': !todo.done
            }"
            @click="handleToggle(todo._id)"
            >{{ todo.done ? "Terminado" : "Pendiente" }}</span
          >
        </h4>
        <p class="todo-date">{{ todo.created | formatDate }}</p>
      </div>
      <div class="buttons">
        <button
          class="btn btn-warning text-white"
          @click="updateTodo(todo._id)"
        >
          Editar
        </button>
        <button
          class="btn btn-danger ml-2"
          @click="todo.done ? deleteTodo(todo._id) : deleteAlert(todo._id)"
        >
          Eliminar
        </button>
      </div>
    </div>
    <p class="todo-desc">{{ todo.description }}</p>
    <Modal
      v-show="showAlert"
      v-on:handleAlert="handleAlert($event)"
      :modal="{
        title: 'Esta seguro de que desea eliminar el todo?',
        desc: 'El todo no esta terminado aun, y no podrá revertir esta acción.'
      }"
    />
  </div>
</template>

<script>
import Modal from "./Layout/Modal";
import baseURL from "../shared/baseURL";
export default {
  name: "todo",
  data() {
    return {
      showAlert: false
    };
  },
  components: { Modal },
  filters: {
    formatDate: function(value) {
      if (!value) {
        return;
      }
      const date = new Date(value);
      const dateFormat = `${date.getDate()}/${date.getMonth()}/${date.getFullYear()}`;
      const hoursFormat = `${date.getHours()}:${
        date.getMinutes() > 10 ? date.getMinutes() : "0" + date.getMinutes()
      }`;
      return `${dateFormat} - ${hoursFormat}`;
    }
  },
  props: ["todo", "todos"],
  methods: {
    deleteAlert: function() {
      this.showAlert = true;
    },
    handleAlert: function(confirmDelete) {
      this.showAlert = false;
      if (confirmDelete) this.deleteTodo(this.todo._id);
    },
    deleteTodo: async function(todoID) {
      try {
        const deletedTodo = await fetch(`${baseURL}/api/todos/${todoID}`, {
          method: "DELETE",
          headers: { "Content-Type": "application/json" }
        });
        const deletedMsg = await deletedTodo.json();
        this.$emit(
          "changeTodo",
          this.todos.filter(todo => todo._id !== todoID)
        );
      } catch (error) {}
    },
    updateTodo: function(todoID) {
      this.$refs.form - todo.focus();
      this.$emit("editTodo", todoID);
    },
    handleToggle: async function(todoID) {
      try {
        await fetch(`${baseURL}/api/todos/toggle-todo/${todoID}`, {
          method: "PUT",
          headers: { "Content-Type": "application/json" }
        });
        this.todo.done = !this.todo.done;
      } catch (error) {}
    }
  }
};
</script>

<style>
.todo {
  padding: 1rem;
  border-radius: 5px;
  margin-bottom: 2rem;
  -webkit-box-shadow: 5px 5px 15px 3px rgba(0, 0, 0, 0.28);
  box-shadow: 5px 5px 15px 3px rgba(0, 0, 0, 0.28);
}

@media (min-width: 600px) {
  .todo-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
  }
}

.badge {
  cursor: pointer;
}

.buttons {
  margin-bottom: 1rem;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  overflow: hidden;
}

.todo-title {
  display: flex;
  align-items: center;
}

.todo-title p {
  margin: 0 0.5rem 0 0;
}

@media (min-width: 600px) {
  .todo-title p {
    max-width: 200px;
  }
}
@media (min-width: 1024px) {
  .todo-title p {
    max-width: 400px;
  }
}
</style>
