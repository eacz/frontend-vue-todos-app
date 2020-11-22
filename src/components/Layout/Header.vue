<template>
  <header class="header">
    <Logo />
    <Nav />
    <button class="btn-delete mr-2" @click="deleteAlert">
      Eliminar todas las tareas
    </button>
    <Modal
      v-show="showAlert"
      v-on:handleAlert="handleAlert($event)"
      :modal="{
        title: 'Esta seguro de que desea eliminar todas las to-dos?',
        desc: 'Esta acción no es reversible.'
      }"
    />
  </header>
</template>

<script>
import Nav from "./Nav";
import Logo from "./Logo";
import Modal from "./Modal";
export default {
  components: { Nav, Logo, Modal },
  data() {
    return {
      showAlert: false
    };
  },
  methods: {
    deleteAlert: function() {
      this.showAlert = true;
    },
    handleAlert: function(confirmDelete) {
      this.showAlert = false;
      if (confirmDelete) this.deleteAllTodos()  
    },
    deleteAllTodos: function() {
      this.$emit("deleteAllTodos");
    }
  }
};
</script>

<style>
.header {
  background-color: #24009c;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-direction: column;
}

@media (min-width: 768px) {
  .header {
    flex-direction: row;
  }
}
.logo {
  width: 100%;
  max-width: 100px;
}
.btn-delete {
  background-color: #0828ff;
  color: #f1f3df;
  border: none;
  border-radius: 5px;
  margin: 0.5rem 0;
  padding: 0.5rem;
}
.btn-delete:hover {
  background-color: #ff2929;
  transition: all 0.3s ease-in-out;
}
</style>
