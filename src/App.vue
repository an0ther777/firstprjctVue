<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>

    <p>Узнать погоду в {{ city == '' ? 'вашем городе' : '- ' + city }}</p>

      <input type="text" v-model="city" placeholder="Введите город">
      <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
      <button disabled v-else>Получить погоду</button>

    <p class="error">{{ error }}</p>


    <div v-if="info !== null">
        <p>{{ showTemp }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

  export default {
    data() {
      return {
        city: '',
        error: '',
        info: null,
      }
    },
    computed: {
      showTemp() {
        return 'Тепература ' + this.info
      }
    },
    methods: {
        getWeather() {
            if(this.city.trim().length < 2){
              this.error = 'Такого города нет :)'
              this.info = null;
              return false
            }
            this.error = ''

            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=a686d36fef80adefeaf8c5d4d22b42a0`)
              .then(res => (this.info = res.data.main.temp))
          }
      }
  }
</script>

<style>
  .error{
    color: #d03939;
  }

  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    text-align: center;
    background-color: #1f0f24;
    color: #fcfcfc;
  }
  .wrapper h1{
    margin-top: 50px;
  }
  .wrapper p{
    margin-top: 20px;
  }
  .wrapper input{
    margin-top: 30px;
    background: #2f1536;
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px;
    outline: none;
    border-radius: 5px;
  }
  .wrapper input:focus{
    border-bottom-color:  rgb(61, 128, 87);
  }
  .wrapper button{
    margin-left: 30px;
    background: #145746;
    border: 0;
    border-radius: 5px;
    color: #fcfcfc;
    padding: 10px 15px; 
    transition: transform 500ms ease;
    cursor: pointer;
  }
  .wrapper button:hover{
    transform: scale(1.1)translateY(-3px);
  }
  .wrapper button:disabled{
    background:  rgb(4,53,138);
    cursor: not-allowed;
  }
  
</style>
