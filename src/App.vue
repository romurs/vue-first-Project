<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город" />
    <button v-show="city != ''" @click="getWether()">Получить погоду</button>
    <P class="error">{{ error }}</P>

    <div v-if="info!=null">
      <p>{{showCityName}}</p>
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return "«" + this.city + "»";
    },
    showCityName(){
      return `Город - ` + this.info.name
    },
    showTemp(){
      return `Температура: ` + this.info.main.temp
    },
    showFeelsLike(){
      return `Ощущается как: ` + this.info.main.feels_like
    },
    showMinTemp(){
      return `Минимальная температура: ` + this.info.main.temp_min
    },
    showMaxTemp(){
      return `Максимальаня температура: ` + this.info.main.temp_max
    },
  },
  methods: {
    getWether() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа!";
        return false;
      }
      this.error = "";

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=855ec0d91a281fe9305691cd26f42322`)
        .then(res => (this.info = res.data))
    },
  },
};

// 855ec0d91a281fe9305691cd26f42322
// https://api.openweathermap.org/data/2.5/weather?q={city name}&units=metric&appid=855ec0d91a281fe9305691cd26f42322
</script>

<style scoped>
.error {
  color: red;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #1f0f24;
  text-align: center;
  color: white;
}
.wrapper h1 {
  margin-top: 50px;
}
.wrapper p {
  margin-top: 20px;
}
.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8 px;
  outline: none;
}
.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}
.wrapper button {
  background: #e3dc4d;
  color: white;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500mx ease;
}
.wrapper button:hover {
  transform: scale(1, 1) translateY(-5px);
}
</style>
