<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <CanvasMove />
  </div>
</template>

<script>
import CanvasMove from './components/CanvasMove.vue'

export default {
  name: 'App',
  components: {
    CanvasMove
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
</style>
