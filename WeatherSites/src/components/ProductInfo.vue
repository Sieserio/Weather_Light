<template>
  <div  class="wrapper-product">
    <p class="prodText">{{showNameProd}}</p>
    <p>{{showCostProd}}</p>
    <button class="btn-delete" @click="deleteProd(index)">Удалить</button>

  </div>

</template>

<script>
  export default {
    props: {
      deleteProd:{
        type: Function
      },
      product:{
        type: Object
      },
      index:{
        type: Number
      }
    },
    computed: {
      showNameProd() {
        return "Продукт :  " + this.product.title
      },
      showCostProd() {
        return "Цена :  " + this.product.cost + ' рублей'
      }
    }
  }
</script>

<style scoped>
  .wrapper-product{
    background: yellowgreen;
    padding: 10px 20px;
    font-size: 16px;
    position: relative;
  }
  .prodText{
    margin-right: 20px;
  }

  .btn-delete{
    padding: 2px 20px;
    background-color: #e09e9e;
    border-radius: 5px;
    border: 1px #d58b8b solid;
    position: absolute;
    right: 10px;
  }
</style>