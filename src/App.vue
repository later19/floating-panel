<template>
  <div id="app">
    <FloatPannel>
      <div>浮动面板</div>
    </FloatPannel>
  </div>
</template>

<script>
import FloatPannel from './components/FloatPannel.vue'

export default {
  name: 'App',
  components: {
    FloatPannel,
  },
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
