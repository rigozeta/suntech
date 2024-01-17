<script setup lang="ts">
import '@progress/kendo-theme-default/dist/all.css';
import Form from './components/Form.vue'
</script>

<template>
  <div class="logoHolder">
    <a href="https://jrcgonzalez.com" target="_blank"><img src="./assets/jrcg-logo.png" class="logo" alt="Vue logo" /></a>
  </div>
  <!-- Render Registration form Component -->
  <Form />
</template>

<style scoped>

.logoHolder {
  text-align: center;
}
.logo {
  height: 6em;
  padding: 1.5em;
}

</style>
