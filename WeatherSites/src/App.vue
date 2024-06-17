<template>
    <div className="wrapper">
      <h1 className="title-big">Погодное приложение </h1>
      <p className="title-medium">{{ city == '' ? 'Впишите ваш город' : cityName}}</p>
      <input type="text" className="input-city" placeholder="Ведите город" v-model="city">
      <button @click="getWeather()" v-if="city!= '' " className="btn">Получить погоду</button>
      <button disabled v-else-if="city == '' " className="btn">Получить погоду</button>
      <p class="title-medium">{{error}}</p>

      <div v-if='info != null' >
        <p  class="title-medium">{{showTemp}}</p>
        <p  class="title-medium">{{showFeels}}</p>
        <p  class="title-medium">{{showMaxTemp}}</p>
        <p  class="title-medium">{{showMinTemp}}</p>
      </div>

    </div>
</template>


<script >
import axios from "axios";
export default {
  data() {
    return {
      city: '',
      error: '',
      info: null,
      products: null,
    }
  },
  computed: {
    cityName() {
      return 'Узнай погоду в ' + '"' + this.city + '"'
    },
    showTemp() {
      return 'Температура: ' + this.info.main.temp
    },
    showFeels() {
      return 'Ощущается как: ' + this.info.main.feels_like
    },
    showMinTemp() {
      return 'Минимальная температура: ' + this.info.main.temp_min
    },
    showMaxTemp() {
      return 'Максимальная температура: ' + this.info.main.temp_max
    },
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = 'Слишком короткое название'
        return false
      }
      this.error =''
          axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`)
          .then(res => (this.info = res.data))
      axios.get('http://localhost:8000/api/products/')
          //.then(inf => (this.products = inf.data))
      //console.log(this.products)
    }
  }
}
</script>

<style scoped>
  .wrapper{
    width: 900px;
    height: 500px;
    border-radius: 20px;
    padding: 20px;
    background: #ffffff;
    text-align: center;
  }

  .title-big{
    margin-top: 40px;
  }

  .title-medium {
    margin-top: 20px;
  }

  .input-city{
    margin-top: 20px;
    border-radius: 10px;
    border: 2px solid #8c8b8b;
    background: transparent;
    font-size: 14px;
    padding: 8px 12px;
    outline: none;
    margin-right: 20px;
  }
  .input-city:focus{
    border-color: #000000;
  }

  .btn{
    background: #4c9ebb;
    padding: 8px 12px;
    border: none;
    outline: none;
    border-radius: 10px;
    cursor: pointer;
    transition: transform 500ms ease;
  }
  .btn:hover{
    transform: scale(1.05);
  }

  .btn:disabled{
    background: #96b1bd;
  }
  .btn:disabled:hover{
    transform: none;
  }
</style>
