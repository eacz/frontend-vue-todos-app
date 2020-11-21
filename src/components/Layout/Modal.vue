<template>
  <div class="modal-m">
    <div class="modal-content-m">
      <div class="modal-header-m">
        <h4>{{ modal.title }}</h4>
      </div>
      <div class="modal-body">
          <p>{{modal.desc}}</p>
      </div>
      <div class="modal-footer-m">
        <button class="btn btn-personalized" @click="confirmDelete">Confimar</button>
        <button class="btn btn-cancel" @click="cancelDelete">Cancelar</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: ["modal"],
  methods: {
      confirmDelete: function(){
          this.$emit('handleAlert', true)
      },
      cancelDelete: function(){
          this.$emit('handleAlert', false)
      }
  }
};
</script>

<style>
.modal-m {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: 1;
  overflow: auto;
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content-m {
    background-color: #f1f3df;
    margin: 15% auto;
    max-width: 600px;
    width: 80%;
    padding: 1rem;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    align-items: center;
}
</style>
