<template>
  <div id="app">
    <h1>Granny Square Blanket Designer</h1>
    <GrannySquareDesigner />
  </div>
</template>

<script>
import GrannySquareDesigner from './components/GrannySquareDesigner.vue'

export default {
  name: 'App',
  components: {
    GrannySquareDesigner
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 0;
  padding: 20px;
}

body {
  margin: 0;
  padding: 0;
  background-color: #f0f0f0;
}

h1 {
  text-align: center;
  margin-bottom: 2rem;
}

button {
  cursor: pointer;
}
</style>