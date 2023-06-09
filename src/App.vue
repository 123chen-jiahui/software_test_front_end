<template>
  <div id="app">
     
    <MainPart/>
    <LeftPart/>
    <HeadPart/>
  </div>
</template>

<script>
import MainPart from './components/MainPart.vue'
import LeftPart from './components/LeftPart.vue'
import HeadPart from './components/HeadPart.vue' 
import Vue from 'vue';
const eventBus = new Vue();
export default {
  name: 'App',
  components: {
    MainPart,
    LeftPart,
    HeadPart
  },
  data(){
    return{
      eventBus,
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;  
  color: #2c3e50;
  margin-top: 60px;
}
</style>




