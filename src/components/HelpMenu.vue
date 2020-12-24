<template>
  <div>
    <h2> What kind of activity are you looking for?<img  v-tooltip.top-center="typeTip" class="helpImage" alt="Help Icon" src="@/assets/help.jpg"></h2>
      <select  v-model="selectedType" >
        <option v-for="types in activityTypes" :key="types">
          <label for="types">{{types}}</label>
        </option>
      </select>    
    <h2 > How many people will participate?<img  v-tooltip.top-center="participantsTip" class="helpImage" alt="Help Icon" src="@/assets/help.jpg"></h2>
      <input  v-model="numberOfParticipants" type="number" id="numberOfParticipants" name="numberOfParticipants" min="1" max="10">

    <h2 > How accessible do you want to do the activity? <img v-tooltip.top-center="accessibilityTip" class="helpImage" alt="Help Icon" src="@/assets/help.jpg"> </h2>  
      <vue-slider v-model="accessibilityRange" :min="0" :max="100" :min-range="0" :height="10" :width="100" :process-dragable="true" tooltip-dir="top"></vue-slider>
    <h2> How much are you willing to pay? <img v-tooltip.top-center="priceTip" class="helpImage" alt="Help Icon" src="@/assets/help.jpg"></h2>
      <vue-slider v-model="priceRange" :min="0" :max="100" :min-range="0" :height="10" :width="100" :process-dragable="true" tooltip-dir="top"></vue-slider>
     
  <br/>
    <h3> {{errorMessage}} </h3>
  <br/>
    <button class="activityButton" @click="getActivity"> Get activity </button>

  <hr>
  </div>
</template>

<script lang="ts" >

import VueSlider from 'vue-slider-component'
import 'vue-slider-component/theme/default.css'
const {ImageRating} = require( 'vue-rate-it');
import Vue from 'vue'
import VTooltip from 'v-tooltip'
import {Component } from 'vue-property-decorator';
 import axios from 'axios';

Vue.use(VTooltip);

@Component({
  components: {
    VueSlider,
    ImageRating,
    
  },
})
export default class HelpMenu extends Vue{
    public activity: any = null;
    public activityTypes: Array<String> = ["education", "recreational", "social", "diy", "charity", "cooking", "relaxation", "music", "busywork"];
    public selectedType: String = '';
    public numberOfParticipants: Number = 1;
    public accessibilityRange: any = [0,100];
    public priceRange: any = [0, 100]
    public errorMessage: String = '';
    public typeTip: String = 'The type of the activity';
    public participantsTip: String = 'The number of people that this activity could involve'; 
    public accessibilityTip: String = 'A factor describing how possible an event is to do with zero being the most accessible';
    public priceTip: String = 'On a percentage scale, how much money do you want to spend on the activity?';

    public getActivity(): void{
        this.activity = null;
        this.errorMessage = '';
        var request ='';

        if(this.selectedType != ''){
          request += 'type='+this.selectedType+'&';
        }
          request += 'participants='+this.numberOfParticipants +'&';

        if(this.accessibilityRange[0] == this.accessibilityRange[1]){
          request += 'accessibility='+this.accessibilityRange[0] + '&';
        }else{
          request += 'minaccessibility='+(this.accessibilityRange[0]/100) + '&maxaccessibility=' + (this.accessibilityRange[1]/100)+ '&';
        }
        if (this.priceRange[0] == this.priceRange[1]){
          request += 'price='+ (this.priceRange[0]/100)
        }else{
          request += 'minprice='+(this.priceRange[0]/100) + '&maxprice=' + (this.priceRange[1]/100)
        }

        axios.get('http://www.boredapi.com/api/activity?'+request )
        .then(res =>{
            if (res.data.error != null){
                this.errorMessage = res.data.error;
                console.log(res.data.error);
                this.activity = null;
                this.$emit("newActivity", this.activity);
                  
            }else{
                this.activity = res.data;
                this.activity.price = (this.activity.price*5)
                this.$emit("newActivity", this.activity);
                this.activity = null;
           }
          

            
        })
        .catch(err => {
            
            console.log(err);
           })
       
      }

  }


</script>


<style >
h1 {
 font-size: 30px;
  color: #004383;
}

h2{
  font-size: 20px;
   margin-bottom: 0;
   color: #8860d0;
   
}
h3{
   font-size: 20px;
   margin-bottom: 0;
   color: #b13535;  
}

.activityButton {
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
.helpImage{
  height: 20px;
}
.vue-slider{
    margin: auto;
}
.ImageRating{
  display: inline-block;
}

.tooltip {
  display: block !important;
  z-index: 10000;
}

.tooltip .tooltip-inner {
  background: #004383;
  color: white;
  border-radius: 16px;
  padding: 5px 10px 4px;
  font-family: sans-serif;
   max-width: 200px;
}

.tooltip .tooltip-arrow {
  width: 0;
  height: 0;
  border-style: solid;
  position: absolute;
  margin: 5px;
  border-color: #004383;
}

.tooltip[x-placement^="top"] {
  margin-bottom: 5px;
}

.tooltip[x-placement^="top"] .tooltip-arrow {
  border-width: 5px 5px 0 5px;
  border-left-color: transparent !important;
  border-right-color: transparent !important;
  border-bottom-color: transparent !important;
  bottom: -5px;
  left: calc(50% - 5px);
  margin-top: 0;
  margin-bottom: 0;
}


.tooltip[aria-hidden='true'] {
  visibility: hidden;
  opacity: 0;
  transition: opacity .15s, visibility .15s;
}

.tooltip[aria-hidden='false'] {
  visibility: visible;
  opacity: 1;
  transition: opacity .15s;
}
</style>
