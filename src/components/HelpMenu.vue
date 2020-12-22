<template>
  <div>
      <h2> What kind of activity are you looking for? </h2>
        <select  v-model="selectedType" >
            <option v-for="types in activityTypes" :key="types">
                <label for="types">{{types}}</label>
            </option>
        </select>    
      <h2> How many people will participate? </h2>
        <input  v-model="numberOfParticipants" type="number" id="numberOfParticipants" name="numberOfParticipants" min="1" max="10">

      <h2> How easily do you want to do the activity? </h2>
        <input   v-model="accessibilityRange" type="range" min="0" max="1" step="0.1" >

      <h2> How much are you willing to pay? </h2>
      <v-range-slider max="93" min="-100"> </v-range-slider>
        <input   v-model="priceRange" type="range" min="0" max="1" step="0.1" >
     <br/>
        <h3> {{errorMessage}} </h3>
        <br/>
       <button @click="getActivity"> Get activity </button>
       <br/>
        <hr>


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
       <input disabled type="range" min="0" max="1"  step="0.01" class="customSlider" v-model="activity.price">

       <hr>
    </div> 
  </div>
</template>

<script lang="ts">

import {Component, Vue } from 'vue-property-decorator';
 import axios from 'axios';

@Component({
  components: {
  },
})

export default class HelpMenu extends Vue{
    public activity: any = null;
    public activityTypes: Array<String> = ["education", "recreational", "social", "diy", "charity", "cooking", "relaxation", "music", "busywork"];
    public selectedType: String = '';
    public numberOfParticipants: Number = 1;
    public priceRange: Number = 0;
    public accessibilityRange: Number = 0;
    public errorMessage: String = '';


    public getActivity(): void{
        this.activity = null;
         this.errorMessage = '';
        var request ='';

        if(this.selectedType != ''){
            request += 'type='+this.selectedType+'&';
        }
        
        request += 'participants='+this.numberOfParticipants +'&';

        if(this.priceRange != 0 && this.accessibilityRange != 0){
            request += 'price='+this.priceRange + '&';
            request += 'accessibility='+this.accessibilityRange
        }else if (this.priceRange == 0 && this.accessibilityRange != 0){
            request += 'accessibility='+this.accessibilityRange
        }else if(this.priceRange != 0 && this.accessibilityRange == 0){
            request += 'price='+this.priceRange;
        }

        axios.get('http://www.boredapi.com/api/activity?'+request )
        .then(res =>{
            if (res.data.error != null){
                this.errorMessage = res.data.error;
                console.log(res.data.error);
            }else{
                this.activity = res.data;
            }
          

            
        })
        .catch(err => {
            
            console.log(err);
           })
       
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
h3{
   font-size: 20px;
   margin-bottom: 0;
   color: #b13535;  
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
img{
  
  height: 75px;
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

</style>
