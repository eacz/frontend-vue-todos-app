<template>
  <div class="filtrado">
    <p>Filtrar por:</p>
    <div class="botones-filtros">
      <button
        @click="changeFilter('default')"
        :class="{ 'filtro-activo': filtrado === 'default' }"
        class="btn-filtro"
      >
        Todos
      </button>
      <button
        @click="changeFilter('terminado')"
        :class="{ 'filtro-activo': filtrado === 'terminado' }"
        class="btn-filtro"
      >
        Terminado
      </button>
      <button
        @click="changeFilter('pendiente')"
        :class="{ 'filtro-activo': filtrado === 'pendiente' }"
        class="btn-filtro"
      >
        Pendiente
      </button>
      <button
        @click="changeFilter('fecha-asc')"
        :class="{ 'filtro-activo': filtrado === 'fecha-asc' }"
        class="btn-filtro"
      >
        Nuevos
      </button>
      <button
        @click="changeFilter('fecha-des')"
        :class="{ 'filtro-activo': filtrado === 'fecha-des' }"
        class="btn-filtro"
      >
        Viejos
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: ["filtrado"],
  methods: {
    changeFilter: function(filtro) {
      this.$emit("changeFilter", filtro);
    }
  }
};
</script>

<style>
.filtrado {
  display: flex;
  flex-wrap: nowrap;
  align-items: center;
  justify-content: space-between;
  background-color: #0828ff;
  margin: 1rem 0;
  padding: 0.5rem;
  border-radius: 5px;
  flex-direction: column;
}
@media (min-width: 500px) {
  .filtrado {
    flex-direction: row;
  }
  .filtrado p {
    margin: 0;
  }
}

.filtrado p {
  font-size: 1.1rem;
  color: #f1f3df;
}

.botones-filtros {
  display: grid;
  grid-template-columns: repeat(3,1fr);
  row-gap: 1rem;
  column-gap: .7rem;
}

@media (min-width: 600px) {
  .botones-filtros {
  margin-left: 1rem;
  display: flex;
  flex-wrap: nowrap;
  justify-content: space-between;
}
}

button:focus {
  outline: none !important;
}

.btn-filtro {
  padding: 0.3rem;
  background-color: #0828ff;
  color: #f1f3df;
  border: 1px solid #f1f3df;
  border-radius: 5px;
  margin-left: 0.3rem;
}



.filtro-activo {
  background-color: #f1f3df;
  color: #0828ff;
  transition: all .5s ease;
  border: none;

}
</style>
