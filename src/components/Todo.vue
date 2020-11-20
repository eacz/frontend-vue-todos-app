<template>
  <div class="todo">
    <div class="todo-header">
      <div class="title">
        <h4 class="todo-title">
          {{ todo.name }}
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
        <button class="btn btn-danger ml-2" @click="deleteTodo(todo._id)">
          Eliminar
        </button>
      </div>
    </div>
    <p class="todo-desc">{{ todo.description }}</p>
  </div>
</template>

<script>
import { local as baseURL } from "../shared/baseURL";
export default {
  name: "todo",
  data() {
    return {};
  },
  filters: {
    formatDate: function(value) {
      if (!value) {
        return;
      }
      const date = new Date(value);
      return `${date.getDate()}/${date.getMonth()}/${date.getFullYear()}`;
    }
  },
  props: ["todo", "todos"],
  methods: {
    deleteTodo: async function(todoID) {
      const deletedTodo = await fetch(`${baseURL}/api/todos/${todoID}`, {
        method: "DELETE",
        headers: { "Content-Type": "application/json" }
      });
      const deletedMsg = await deletedTodo.json();
      this.$emit(
        "changeTodo",
        this.todos.filter(todo => todo._id !== todoID)
      );
    },
    updateTodo: async function(todoID) {
      this.$emit("editTodo", todoID);
    },
    handleToggle: async function(todoID) {
      await fetch(`${baseURL}/api/todos/toggle-todo/${todoID}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" }
      });
      this.todo.done = !this.todo.done;
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
  justify-content: flex-end;
}
</style>
