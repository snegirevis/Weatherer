<script >
import { getTransitionRawChildren } from 'vue';
import axios from 'axios';

export default{
  data(){
    return{
    city:"",
    error:"",
    info:null
  }
},

  methods:{
   getWeather(){

     if(this.city.trim().length<2){
      this.error="The name of the city should be more than 1 symbol";
      return false;
     }
     this.error="";

     axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=b3edfa77c33432d16cdaa7703eaa2b7d`)
       .then(res=> (this.info=res.data))
   }
  },

  computed:{
    cityName(){
      return "<" + this.city + ">" 
    },
    showTemp(){
      return "Temperature: " + this.info.main.temp
    },
    feelsLike(){
      return "Feels like: " + this.info.main.feels_like
    },
    minTemp(){
      return "Minimal temperature: " + this.info.main.temp_min
    },
    maxTemp(){
      return "Maximal temperature: " + this.info.main.temp_max
    }
    }
  }
</script>

<template>
    <div class="wrapper">
      <h1>Weather application</h1>
      <p >Get to know with the weather in {{ city== "" ? "your city" :cityName }}</p>
      <input v-model="city" type="text" placeholder="Enter your city">
      <button v-if="city!=''" @click="getWeather">Get the weather</button>
      <button disabled v-else>Enter your city</button>
      <p class="error" v-show="error!=''">{{ error }}</p>

      <div class="data" v-if="info!=null">
      <p>{{ showTemp }}</p>
      <p>{{ feelsLike }}</p>
      <p>{{ minTemp }}</p>
      <p>{{ maxTemp }}</p>
      
      </div>
    </div>
</template>

<style scoped>
.wrapper{
  background-color: #1f0f24;
  height: 50vh;
  border-radius: 50px;
  text-align: center;
  color: #ECEBF3;
  width: 40vw;
}
.wrapper h1{
  padding-top: 3vw;
  padding-bottom: 0.8vw;
}
.wrapper p{
  color: #c3c3c3;
}
.wrapper input{
  margin-top: 10px;
  background: transparent;
  border: 0;
  color: white;
  padding: 5px 8px;
  outline: none;
  border-bottom: 2px solid black;
}
.wrapper input:focus{
  border-bottom-color: #61a47c;
}

.wrapper button{
  border-radius: 10px;
  border: 2px solid #61a47c ;
  padding: 5px ;
  margin-left: 20px;
  cursor: pointer;
  background-color:#61a47c ;
  color: white;
  transition: transform 500ms ease;
  }

  .wrapper button:hover {
    transform: scale(1.05) translateY(-2px);
  }

  .wrapper button:disabled{
   cursor: not-allowed;
   background-color: #578f6d;
  }
  .error{
    color: #c3c3c3;
    margin-top: 0.8vw;
  }

  .data{
    margin-top: 2vw;
  }
</style>
