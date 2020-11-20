<template>
  <div>
    <header class="header">
      <Logo />
      <Nav />
    </header>
  </div>
</template>

<script>
import Nav from "./Nav";
import Logo from "./Logo";
export default {
  components: { Nav, Logo },
  data() {
    return {};
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
</style>
