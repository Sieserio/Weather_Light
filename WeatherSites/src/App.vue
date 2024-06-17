<template>
    <div class="wrapper">
      <h1 class="title-big">Погодное приложение </h1>
      <p class="title-medium">{{ city === '' ? 'Впишите ваш город' : cityName}}</p>
      <input type="text" class="input-city" placeholder="Ведите город" v-model="city">
      <button @click="getWeather()" v-if="city!== '' " class="btn">Получить погоду</button>
      <button disabled v-else-if="city === '' " class="btn">Получить погоду</button>

      <p class="title-medium">{{error}}</p>

      <div v-if='info != null' >
        <p  class="title-medium">{{showTemp}}</p>
        <p  class="title-medium">{{showFeels}}</p>
        <p  class="title-medium">{{showMaxTemp}}</p>
        <p  class="title-medium">{{showMinTemp}}</p>
      </div>

    </div>

  <div class="wrapper wrapper-big">
    <h1 class="title-big">Покупки </h1>
    <p class="title-medium">{{ productName === '' ? 'Впишите продукт' : productNames}}</p>
    <input type="text" class="input-city" placeholder="Ведите продукт" v-model="productName">
    <button v-if="productName !== '' " @click="getProduct(productName, productCost)" class="btn">Получить продукт</button>
    <button disabled v-else-if="productName === '' " @click="getProduct(productName)" class="btn">Получить продукт</button>
    <ProductInfo
        class="prodInfo"
        v-show="prodHas === true"
        :prodName="productName"
        :prodCost="productCost"
        :deleteProd="deleteProd"/>
  </div>
</template>


<script >
import axios from "axios";
import ProductInfo from "@/components/ProductInfo.vue";
export default {
  components: {ProductInfo},
  data() {
    return {
      city: '',
      error: '',
      info: null,
      products: '',
      productName: '',
      productCost: 0,
      prodHas: false,
      costProduct: [],
    }
  },
  computed: {
    cityName() {
      return 'Узнай погоду в ' + '"' + this.city + '"'
    },
    productNames() {
      return 'Ты хочешь купить ' + this.productName
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

    },
    getProduct(productName, productCost, has) {
      axios.get('http://localhost:8000/api/products/')
          .then(inf => (this.products = inf.data))
      for (let i = 0; i<this.products.length; i++) {
        if (productName == this.products[i].title) {
          console.log(productName)
          this.prodHas = true
          this.productCost = this.products[i].cost
          return;
        }
      }
      console.log('не нашел')
      this.prodHas = false
    },
    deleteProd() {

    }
  }
}


</script>

<style scoped>
  .wrapper{
    width: 900px;
    height: 200px;
    border-radius: 20px;
    padding: 20px;
    margin-bottom: 20px;
    background: #ffffff;
    text-align: center;
  }
  .wrapper-big{
    height: 400px;
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

  .prodInfo{
    display: flex;
    margin-top: 20px;
  }
</style>
