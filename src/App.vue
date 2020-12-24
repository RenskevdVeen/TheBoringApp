<template>
  <div id="app">
    <img alt="Boring logo" src="@/assets/panda.png">
     <h1>The Boring App</h1>
    <h2>Got nothing ToDo? We got you!</h2>
    <hr>
    <div v-if="this.component == 'HelpMenu'">
      <HelpMenu @newActivity="updateActivity"/>
     
    </div>
        <MainOverview :activity="this.activity"/>
     <div v-if="this.component == 'MainOverview'"> 
      
        <button @click="getActivity"> Get random activity </button>
     </div>
    <br/>
    <img @click="getHelpMenu" class="helpPageImage" alt="Help logo" v-bind:src="require(`../src/assets/${this.imageSource}`)"/>

  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import MainOverview from './components/MainOverview.vue';
import HelpMenu from './components/HelpMenu.vue';
import axios from 'axios';

@Component({
  components: {
    MainOverview,
    HelpMenu,
  },
})

export default class App extends Vue {
  public component : String = "MainOverview"
  public imageSource: String = "help.jpg"
  public activity: any = null;

  public updateActivity(activity:any){
    this.activity = activity
  }

  public getActivity(): void{
        axios.get('http://www.boredapi.com/api/activity/')
        .then(res =>{
          this.activity = res.data;
          this.activity.price = (this.activity.price*5)
        })
        .catch(err => console.log(err));
  }

  public getHelpMenu():void{
    this.activity = null;
      if(this.component === "HelpMenu"){
        this.imageSource = "help.jpg"
        this.component = "MainOverview";
        
      }else{
        this.imageSource = "close.png"
        this.component = "HelpMenu";
          
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
button {
  height: 50px;
  width: 200px;
    border-radius: 25px;
   border-radius: 25px;
   border: 2px solid  #004383;
   outline: 0;
  background:  #294ca4;
  color: white;
  font-size: 15px;
  
}

button:hover {
  cursor: pointer;
}

img{
  height: 10%;
  width: 20%;
}
.helpPageImage{
  margin-top: 1%;
  width: 40px;
  height: 40px;
}

.helpPageImage:hover {
  cursor: pointer;
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
