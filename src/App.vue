<script setup>
import Navbar from "./components/Navbar.vue";

</script>

<template>
  <section class="container">
    <Navbar class="nav" title="WIKI COUNTRIES"></Navbar>

    <div class="container-app">
      <router-view></router-view>
    </div>
  </section>
</template>

<style scoped>
.container {
  max-height: 100vh;
  height: 100vh;
  
  .container-app {
    width: 100%;
    height: 80%;
    display: flex;
    justify-content: space-evenly;
    margin-top: 20px
  }
}

@media (max-width: 800px) {
  .container-app {
    width: 100%;
    height: 100%;
    flex-direction: column;
    align-items: center;
    gap: 20px;
  }
}
</style>
