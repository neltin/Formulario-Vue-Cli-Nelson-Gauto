<template>
  <div id="app">
    <img class="logo-small" alt="Vue logo" src="./assets/logo.png">
    <FormularioPage />
  </div>
</template>

<script>
import FormularioPage from './components/FormularioPage.vue'

export default {
  name: 'App',
  components: {
    FormularioPage
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.logo-small {
 width: 6em;
}
</style>
