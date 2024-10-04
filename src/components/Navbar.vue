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
  margin-bottom: 40px;

  .title {
    font-size: 20px;
    color: white;
    margin-left: 10px;
  }
}
</style>
