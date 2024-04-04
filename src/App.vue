<script>
import axios from 'axios'

export default{
  data() {
    return{
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName(){
      return "«" + this.city + "»"
    },
    showTemp(){
      return "<b>Температура сейчас:</b> " + this.info.main.temp + " °С"
    },
    showFeelslike(){
      return "<b>Ощущается как:</b> " + this.info.main.feels_like + " °С"
    },
    showMintemp(){
      return "<b>Минимальная температура:</b> " + this.info.main.temp_min + " °С"
    },
    showMaxtemp(){
      return "<b>Максимальная температура:</b> " + this.info.main.temp_max + " °С"
    },
    showWindspeed(){
      return "<b>Скорость ветра: </b>" + this.info.wind.speed + " м/с"
    },
    showClouds(){
      return "<b>Облака: </b>" + this.info.clouds.all + " %"
    },
    showGlobalweather(){
      return "<b>Описание: </b>" + this.info.weather[0].description
    },
    showCoors(){
      return "<b>Координаты:</b> [" + this.info.coord.lon + ", " + this.info.coord.lat + "]"
    }
  },
  methods: {
    getWeather(city){
      if (city.trim().length < 2) {
        this.error = "Нужно название более одного символа"
        return false
      }
      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=3a8788344fce8b7cc09d0e106f7c0e97`)
           .then(res => (this.info = res.data))
    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город" @keyup.enter="getWeather(city)">
    <button v-if="city != ''" @click="getWeather(city)">Получить погоду</button>
    <button disabled v-else>Введите название города</button>
    <p class="error">{{ error }}</p>
    <div v-if="info != null" >
      <p v-html="showCoors" class="weather_locate"></p>
      <p v-html="showGlobalweather"></p>
      <p v-html="showTemp"></p>
      <p v-html="showFeelslike"></p>
      <p v-html="showMintemp"></p>
      <p v-html="showMaxtemp"></p>
      <p v-html="showWindspeed"></p>
      <p v-html="showClouds"></p>
    </div>
  </div>
</template>

<style scoped>

.error{
  color: #d03939;
}

.wrapper{
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #040139;
  text-align: center;
  color: #fff;
}

.wrapper h1{
  margin-top: 30px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input{
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus{
  border-bottom-color: #8ad6ff;
}

.wrapper button{
  background: #8ab9ff;
  color: #fff;
  border-radius: 10px;
  border: 2px  solid #8ab9ff;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover{
  transform: scale(1.1) translateY(-5px);
}

.wrapper button:disabled{
  background: #7197d0;
  cursor: not-allowed;
}
</style>
