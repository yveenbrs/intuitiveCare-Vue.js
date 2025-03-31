<template>
  <div id="app">
    <SearchOperadoras/>
  </div>
</template>

<script>
import SearchOperadoras from './components/SearchOperadoras.vue'

export default {
  name: 'App',
  components: {
    SearchOperadoras
  },
  mounted() {
    document.title = "Buscar Operadoras - CADOP";
  }
}
</script>

<style>
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  background-color: #f4f4f9;
  color: #333;
}
</style>
