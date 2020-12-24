<template>
  <div>
     <div v-if="activity != null">
      <h1> {{activity.activity}} </h1>
      <h2> Accessibility </h2>
      <input disabled type="range" min="0" max="1" step="0.01" class="customSlider" :value="activity.accessibility">
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
        :rating="activity.price" 
        v-bind:src="require(`../assets/coins.png`)">
      </image-rating>
       </div>
       <hr>
    </div> 
  </div>
</template>

<script lang="ts">


import {Component, Vue, Prop, Watch} from 'vue-property-decorator';
const {ImageRating} = require( 'vue-rate-it');


@Component({
  components: {
    ImageRating
  },
})
export default class MainOverview extends Vue{
@Prop() activity: any = null;

@Watch('newActivity')
activityChanged(newActivity: any) {
  this.activity = newActivity
}
  

}
</script>


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
