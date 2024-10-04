<script setup>
import Navbar from './components/Navbar.vue'

</script>
<template>
  <section class="container">
  <Navbar title="WIKI COUNTRIES"></Navbar>
  <div class="app">
    <div class="container-app">
      <router-view></router-view>
    </div>
  </div>
  </section>
</template>
<style scoped>
  .app {
    display: flex;
    flex-direction: row;
    align-items: center;
    align-content: normal;
    width: 100%;
    height: 80%;
  }
  .container-app {
    display: flex;

    justify-content: space-evenly;
    align-items: center;
    align-content: normal;
  }

  .container{
    max-height: 100vh;
    height: 100vh;
  }
</style>