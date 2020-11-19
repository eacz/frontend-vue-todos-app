<template>
  <nav class="nav">
    <li><a href="#">Todas las tareas</a></li>
    <!--<li><a href="#">Tareas Urgentes</a></li> -->
    
  </nav>
</template>

<script>
export default {};
</script>

<style>
.nav {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav li {
  list-style: none;
  margin-right: 1rem;
}

.nav li a {
  text-decoration: none;
  color: #f1f3df;
}
</style>
