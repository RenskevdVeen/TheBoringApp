<template>
  <div>
     <div v-if="this.activity != null">
      <h1> {{activity.activity}} </h1>
      <h2> Accessibility </h2>
      <input disabled type="range" min="0" max="1" step="0.01" class="customSlider" v-model="activity.accessibility">
      <h2> Type </h2>
      <label> {{activity.type}} </label>
      <h2> Number of participants </h2>
      <div v-if="activity.participants < 2">
            <img src="@/assets/single.png">
      </div>
      <div v-else-if="activity.participants > 1 && activity.participants < 5">
            <img src="@/assets/double.png">
      </div>    
      <div v-else>
        <img src="@/assets/triple.png">
      </div>
      <h2> Price </h2>
      <div class="ImageRating">
      <image-rating 
        :read-only=true 
        :show-rating=false 
        :increment= 0.5
        :item-size= 40
        v-model="activity.price" 
        v-bind:src="require(`../assets/coins.png`)">
      </image-rating>
       </div>

       <hr>
    </div> 
   
    <button @click="getActivity"> Get random activity </button>
    <br/>
  </div>
</template>

<script lang="ts">

import axios from 'axios';
import {Component, Vue } from 'vue-property-decorator';
const {ImageRating} = require( 'vue-rate-it');

@Component({
  components: {
    ImageRating
  },
})
export default class MainMenu extends Vue{
  public activity: any = null;

  public getActivity(): void{
        axios.get('http://www.boredapi.com/api/activity/')
        .then(res =>{
          this.activity = res.data;
          this.activity.price = (this.activity.price*5)

        })
        .catch(err => console.log(err));
      }
  }


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

h1 {
 font-size: 30px;
 
  color: #004383;
}
h2{
  font-size: 20px;
   margin-bottom: 0;
   color: #8860d0;
}
label{
  font-size: 20px;
  color: #294ca4;
}
img{
  
  height: 75px;
}
.helpImage{
  margin-top: 10px;
  height: 40px;
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
.customSlider{
   -webkit-appearance: none;
  background: linear-gradient(to right, lightblue, rgb(94, 231, 224), blue);
   border-radius: 25px;
   outline: 0;
}

.customSlider::-webkit-slider-thumb{
 -webkit-appearance: none;
  appearance: none;
  width: 10px;
  height: 15px;
  background:  #004383;
   border-radius: 25px;
   
}
.ImageRating{
  display: inline-block;
}
</style>
