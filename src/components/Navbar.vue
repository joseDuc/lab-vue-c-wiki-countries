<script setup>
import { defineProps } from "vue";

const props = defineProps({
  title: {
    type: String,
  },
});
</script>

<template>
  <div class="navbar">
    <h1 class="title">{{ title }}</h1>
  </div>
</template>

<style scoped>
.navbar {
  background-color: blue;
  
  .title {
    font-size: 20px;
    color: white;
    width: 100%;
    margin-left: 10px;
    text-align: center;
  }
}

</style>
