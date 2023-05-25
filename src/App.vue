<script>
import axios from 'axios'
  export default {
    data () {
      return {
        city: '',
        error: '',
        info: null
      }
    },
    computed: {
      cityName () {
        return '<' + this.city + '>'
      },
      showTemp() {
        return 'Температура: ' + this.info.main.temp
      },
      showFeelsLike() {
        return 'Ощющается как: ' + this.info.main.feels_like
      },
      showMinTemp() {
        return 'Минимальная температура: ' + this.info.main.temp_min
      },
      showMaxTemp() {
        return 'Максимальная температура: ' + this.info.main.temp_max
      },
    },
      
    // computed свойство возвращяет некий код который можно использовать в разметке
    methods: {
      getWeather () {
        if(this.city.trim().length < 3) {
          // trim() обрезает пробелы
          this.error = 'Введите больше символов'
          return false
        }

        this.error = ''

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=0820ddcc77cb378e8aba25a263679c1f`)
            .then(res => (this.info = res.data))
      }
    }
    // methods - это функции
  }
</script>

<template>
  <div class="wrapper">
    <h1>Приложение для просмотра погоды</h1>
    <p>Узнать погоду в  {{ city == '' ? 'вашем городе' :  cityName}}</p>
    <input type="text" v-model="city" placeholder="введите город">
    <!-- v-model сразу устанавливает значение в переменную city при изменении поля input -->
    <!-- <button v-show="city != ''">получить погоду</button> -->
    <!-- v-show показывает данный тег если условие верное если нет то скрывает его -->
    <button v-if="city != ''" @click="getWeather()">получить погоду</button>
    <button disabled v-else="city != ''">Введите название города</button>
    <p class="error">{{ error }}</p>
    <div class="result" v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
    
  </div>
</template>

<style scoped>
 .wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background-color: #1f0f24;
  text-align: center;
 }
 .wrapper h1 {
  margin-top: 50px;
 }
 .wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: white;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
  transition: 0.3s ease;
 }
 .wrapper button {
  background: #e3bc4b;
  color: white;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 0.5s ease;
 }
 .wrapper button:disabled {
  background: #6d6d6d;
  cursor: not-allowed;
 }
 .wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
 }
 .wrapper input:hover {
  border-bottom-color: #6e2d7d;
 }
 .error {
  color: red
 }
 .result {
  margin-top: 25px;
 }
 .result p{
  margin-top: 25px;
 }
</style>
