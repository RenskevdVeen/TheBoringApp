<template>
  <div class="hello">
     <div v-if="this.activity != ''">
      <h1> {{this.activity}} </h1>
      <h2> Accessibility </h2>
      <label> {{this.accessibility}} </label>
      <input type="range" min="0" max="1" step="0.01" class="accessibilitySlider" v-model="this.accessibility">
      <h2> Type </h2>
      <label> {{this.type}} </label>
      <h2> Number of participants </h2>
      <label> {{this.participants}} </label>
      <div v-if="this.participants < 2">
            <img src="@/assets/single.png">
      </div>
      <div v-else-if="this.participants > 1 && this.participants < 5">
            <img src="@/assets/double.png">
      </div>    
      <div v-else>
        <img src="@/assets/triple.png">
      </div>
      <h2> Price </h2>
      <label> {{this.price}} </label>
       <input type="range" min="0" max="1" step="0.01" class="accessibilitySlider" v-model="this.price">
      <h2> key </h2>
      <label> {{this.key}} </label>
    </div> 
    <button @click="getActivity"> Get random acitivty </button>
   
  </div>
</template>

<script lang="ts">

import axios from 'axios';
import {Component, Vue } from 'vue-property-decorator';

@Component
export default class MainMenu extends Vue{
    public activity: String = "";
    public accessibility: Number = 0;
    public type: String = "";
    public participants: Number = 0;
    public price: Number = 0;
    public key: Number = 0;


  public getActivity(): void{
        axios.get('http://www.boredapi.com/api/activity/')
        .then(res =>{
          this.activity = res.data.activity;
          this.accessibility = res.data.accessibility;
          this.type = res.data.type;
          this.participants = res.data.participants;
          this.price = res.data.price;
          this.key = res.data.key;
        })
        .catch(err => console.log(err));
      }
  }


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  margin: 40px 0 0;
}
img{

  height: 100px;
}
button {
  height: 50px;
  width: 200px;
   border-radius: 25px;
   outline: 0;
    margin-top: 10%;
  
}
.accessibilitySlider{
   -webkit-appearance: none;
  background: linear-gradient(to right, green, orange, red);
   border-radius: 25px;
   outline: 0;
}

.accessibilitySlider::-webkit-slider-thumb{
 -webkit-appearance: none;
  appearance: none;
  width: 5px;
  height: 25px;
  cursor: pointer;
  background: black;
   border-radius: 25px;
   
}
</style>
