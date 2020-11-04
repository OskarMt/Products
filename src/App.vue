<template>
  <div id="app">
   <Preloader/>
    <Product v-for="index in count" :key="index" :productCounter="count-1"/>
  </div>
</template>

<script>

import Product from './components/product.vue'
import Preloader from './components/preloader.vue'


export default {
  data(){
      return{
           count:1
      }

  },
    /*Отображение 20 товаров*/
  beforeMount(){
    this.interval = setInterval(() => {
        if(this.count < 20){
            this.count++
        }
    },1)
  },
  name: 'App',
  components: {
    Product,
    Preloader,
  }
}
</script>

<style>
#app {
    width: 100%;
    max-width: 1000px;
    margin: 0 auto;
}

    @import '../src/style/css/style.css';
</style>
