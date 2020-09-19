<template>
  <div id="app">
    <HelloWorld v-if="true" msg="Welcome to Your Vue.js App"/>
    <Border v-else/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Border from './components/Border';

export default {
  name: 'App',
  components: {
    HelloWorld,
    Border
  }
}
</script>

<style>
*{
    margin:0;
    padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
