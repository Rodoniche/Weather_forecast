<script>
import axios from 'axios'

export default {
    data() {
      return {
        city: "",
        error: "",
        info: null
      }
    },
    computed: {
      cityName() {
        return (this.city)
      }
    },
    methods: {
      getweather() {
        if (this.city.trim().length < 2) {
          this.error = "Нужно название более одного символа"
          this.info = null
          return false
        }
        this.error = ''

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=31970c525ade82ca57f1423c4780f333`)
            
            .then(res => (this.info = res.data.main.temp))
            .catch(res => (this.info = 'Неудачный запрос'))
        
      }
    }
}
</script>

<template>
  <div className="wrapper">
    <h1> Погода</h1>
    <p>узнать погоду в {{ city == '' ? "Вашем городе": cityName}}</p>
    <input type="text" v-model="this.city" placeholder="Введите город" @keyup.enter="getweather()()">
    <button v-if="city != ''" @click="getweather()">Получить погоду</button>
    <button disabled v-else>Введите название города</button>
    <p class="error">{{ error }}</p>
    <p v-show="info != null && info != 'Неудачный запрос'">Температура {{ info }}</p>
    <p v-show="info == 'Неудачный запрос'">Неудачный запрос</p>
  </div>
</template>

<style scoped>
.error {
  color: red;
}
.wrapper {
  width:900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background-color: rgb(255, 255, 255);
  text-align:center;
}
.wrapper h1{
margin-top: 50px;
}
.wrapper p {
  margin-top: 20px;
}
.wrapper button:disabled {
  background: blue;
  cursor: not-allowed;

}
.wrapper input {
  margin-top: 30px;
  background-color: transparent;
  border-bottom: 2px solid rgb(149, 163, 201);
  border: 2px solid;
  border-color: rgb(255, 255, 255);
  color: black;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;

}

.wrapper input:focus {
  border-bottom-color:blue;
}
.wrapper button {
  background: blue;
  color:white;
  border-radius: 10px;
  border: 2px solid blue;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}
.wrapper button:hover{
  transform: scale(1.1) translateY(-5px);
}
</style>
