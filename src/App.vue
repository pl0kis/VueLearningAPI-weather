<script>
import axios from 'axios';

  export default {
    data(){
      return{
        city:"",
        error:"",
        info:null
      }
    },
    computed:{
      cityName(){
        return "'" + this.city + "'"
      },
      showTemp(){
        return "Температура: " + this.info.main.temp
      },
      showFeelsLike(){
        return "Ощущается как: " + this.info.main.feels_like
      },
      showMinTemp(){
        return "Минимальная температура: " + this.info.main.temp_min
      },
      showMaxTemp(){
        return "Максимальная температура: " + this.info.main.temp_max
      },
      
    },
    methods:{
      getWeather(){
        if(this.city.trim().length < 2){
          this.error ='Нужно название больше одного символа'
          return false
        }
        this.error = ''
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=d755a68c85cb29a1e9d9bfb25d0f0b47`)
        .then(res => {this.info = res.data})
      }
    }
  }

</script>

<template>
  <div className="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город">
    <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите название города</button>
    <p className="error">{{ error }}</p>

    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
    
  </div>
</template>

<style scoped>
.error{
  color: red;
}
.wrapper{
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background-color: #1f0f24;
  text-align: center;
  color: white
}
.wrapper h1{
  margin-top: 50px;
}
.wrapper p{
  margin-top: 20px;
}
.wrapper input{
 margin-top: 30px;
 background-color: transparent;
 border:0;
 border-bottom: 2px solid #110813;
 color: #fcfcfc;
 font-size: 14px;
 padding: 5px 8px;
 outline: none;
 transition: 0.2s;

}
.wrapper input:focus{
  border-bottom-color: #6e2d7d;
  transition: 0.2s;
}
.wrapper button:disabled{
  background-color: #392e11;
  cursor:grab;
}

.wrapper button{
  background-color: #e6bc4b;
  color:#fff;
  border-radius:10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}
.wrapper button:hover{
  transform: scale(1.1) translateY(-5px);
}
</style>
