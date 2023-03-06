<template>
  <div id="app">
	<Canvas/>
  </div>
</template>

<script>
import Canvas from './components/Canvas.vue'
export default {
  name: 'App',
components: {
	//use Canvas.vue
	Canvas

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
  position: absolute;
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
  background: #2c3e50;
  top: 0px;
  left: 0px;

}
</style>
