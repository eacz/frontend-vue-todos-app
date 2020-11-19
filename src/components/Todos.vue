<template>
  <div class="todos">
    <h1>To-dos</h1>
    <Todo
      :todos="todos"
      v-for="todo in todos"
      :todo="todo"
      :key="todo._id"
      v-on:changeTodo="updateTodo($event)"
      v-on:editTodo="editTodo($event)"
    ></Todo>
  </div>
</template>

<script>
import Todo from "./Todo";
export default {
  name: "todos",
  components: { Todo },
  data() {
    return {};
  },
  props: ["todos"],
  methods: {
    updateTodo: function(updatedTodos) {
      this.$emit("updateTodo", updatedTodos);
    },
    editTodo: function(todoID){
        this.$emit('editTodo', todoID)
    }
  }
};
</script>

<style>
.todos {
}
</style>
