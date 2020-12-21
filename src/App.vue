<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
     <h1>The Boring App</h1>
    <h2>Got nothing ToDo? We got you!</h2>
    <hr>

    <MainMenu />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import MainMenu from './components/MainMenu.vue';

@Component({
  components: {
    MainMenu,
  },
})
export default class App extends Vue {}
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
