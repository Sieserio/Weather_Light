<template>
  <div class="wrapper-product">
    <p class="prodText">{{showNameProd}}</p>
    <p>{{showCostProd}}</p>
    <button @click="deleteProd">Удалить</button>
  </div>
</template>

<script>
  export default {
    props: {
      prodName: {
        type: String
      },
      prodCost: {
        type: Number
      },
      deleteProd:{
        type: Function
      }
    },
    computed: {
      showNameProd() {
        return "Продукт :  " + this.prodName
      },
      showCostProd() {
        return "Цена :  " + this.prodCost + ' рублей'
      }
    }
  }
</script>

<style scoped>
  .wrapper-product{
    background: yellowgreen;
    padding: 10px 20px;
    font-size: 16px;
  }
  .prodText{
    margin-right: 20px;
  }
</style>