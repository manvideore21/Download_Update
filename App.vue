<template>
  <GoodName greeting="Hello, Manvi!" />
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld />
  
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import GoodName from './components/GoodName.vue';

export default {
  name: 'App',
  components: {
    GoodName,
    HelloWorld
    
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
