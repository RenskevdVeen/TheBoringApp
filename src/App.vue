<template>
  <div id="app">
    <img alt="Boring logo" src="@/assets/panda.png">
     <h1>The Boring App</h1>
    <h2>Got nothing ToDo? We got you!</h2>
    <hr>
    
    <component v-bind:is="component"/>
 
    <img @click="getHelp" class="helpPageImage" alt="Help logo" v-bind:src="require(`../src/assets/${this.imageSource}`)"/>

  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import MainMenu from './components/MainMenu.vue';
import HelpMenu from './components/HelpMenu.vue';

@Component({
  components: {
    MainMenu,
    HelpMenu,
  },
})

export default class App extends Vue {
  public component :any = "MainMenu"
  public imageSource: String = "help.jpg"

  

  public getHelp():void{
      if(this.component === HelpMenu){
        this.imageSource = "help.jpg"
        this.component = MainMenu;
        
      }else{
        this.imageSource = "close.png"
        this.component = HelpMenu;
          
      }
      
  }
}
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  
}
img{
  width: 20%;
}
.helpPageImage{
  margin-top: 1%;
  width: 40px;
  height: 40px;
}

h1{
  margin: 0;
  color: #004383;
  font-size: 40px;
}
h2{
  margin: 0;
  font-size: 20px;
  color: #8860d0;
}
</style>
