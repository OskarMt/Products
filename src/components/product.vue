<template>
    <div class="wrapper">
        <div class="product-description">
            <img v-bind:src="this.imageModificator(this.productImg)" class="product__img">
            <div class="product-text">
                <div class="product__code">Код: {{productCode}}</div>
                <h2 class="product__title">{{title}}</h2>
                <p class="product__description">{{description}}</p>
                <p class="product__weight">Вес: {{weight}}</p>
                <p class="product__required"><b>Могут понадобиться:</b> {{productRequired}}</p>
            </div>
        </div><!--/Product-description-->
        <div class="product-buy">
            <div class="product-price-gold">
                <p>По карте <br>клуба</p>
                <h2 class="price__Gold">{{priceGold}}</h2>
            </div>
            <h2 class="price__Retail">{{priceRetail}}</h2>
            <div class="product-btns">
                <span class="product__meter" @click="useMeter()" :class="{active: useMeterUnits}">За {{priceUnitAlt}}</span>
                <span class="product__pack" @click="usePack()" :class="{active: !useMeterUnits}" v-if="showAllUnits">За {{priceUnit}}</span>
            </div>
            <div class="product-counter">
                <div class="counter"><b>Количество: {{counter}}</b></div>
                <div class="product-counter-btns">
                    <button @click="removeCount()"><img src="../assets/minusIcon.png" alt=""></button>
                    <button @click="addCount()"><img src="../assets/plusIcon.png" alt=""></button>
                </div>
            </div><!--/Product-counter-->
            <div class="buy-btn">
                <a href="#" v-bind:data-product-id="productId" class="buy__btn">В корзину</a>
            </div>
        </div><!--/Product-buy-->
    </div><!--/Wrapper-->
</template>


<script>
import Products from '../../products.json'

export default{
    props:{
       productCounter: Number,
    },
    data(){
        return {

                /*Описание товара*/
                title: Products[this.productCounter].title,
                description: Products[this.productCounter].description,
                productRequired:Products[this.productCounter].assocProducts,
                productImg:Products[this.productCounter].primaryImageUrl,
                productCode:Products[this.productCounter].code,
                weight:Products[this.productCounter].weight,

                /*Цены и единицы измерения*/
                priceGold: (Products[this.productCounter].priceGoldAlt).toFixed(2) + ' Р',
                priceRetail:(Products[this.productCounter].priceRetailAlt).toFixed(2) + ' Р',
                priceUnit:Products[this.productCounter].unit,
                priceUnitAlt:Products[this.productCounter].unitAlt,

                productId:Products[this.productCounter].productId,


                useMeterUnits: true,
                showAllUnits:true,
                counter: 1,
                productIndex: 0

            }
    },
  methods:{
      /*Добавление и убавление количества товара*/
      addCount(){
        this.counter++
        if(this.useMeterUnits){
            this.updateMeterPrice();
        }else{
            this.updatePackPrice();
        }
      },
       removeCount(){
         if(this.counter > 1){
           this.counter--
                if(this.useMeterUnits){
                    this.updateMeterPrice();
                }else{
                    this.updatePackPrice();
                }
         }
      },
      /*Кнопки переключения цены */
      useMeter(){
          this.useMeterUnits = true;
          this.updateMeterPrice();
      },
      usePack(){
          this.useMeterUnits = false;
          this.updatePackPrice();
      },
      /*Обновление цены для упак.*/
      updatePackPrice(){
          this.priceGold = (Products[this.productIndex].priceGold * this.counter).toFixed(2) + ' Р';
          this.priceRetail = (Products[this.productIndex].priceRetail * this.counter).toFixed(2)  + ' Р';
      },
      /*Обновление цены для м.кв.*/
      updateMeterPrice(){
          this.priceGold = (Products[this.productIndex].priceGoldAlt * this.counter).toFixed(2) + ' Р' ;
          this.priceRetail = (Products[this.productIndex].priceRetailAlt * this.counter).toFixed(2) + ' Р';
      },
      imageModificator(image){
          let imageName = image.split('.');
          imageName[imageName.length - 2] = imageName[imageName.length - 2] + '_220x220_1';
          return imageName.join('.')
      }
  },
  beforeMount(){
      this.productIndex = this.productCounter;

      /*Если товар имеет только один вариант продажи, показывать только его*/
      if(this.priceUnitAlt == this.priceUnit){
        this.showAllUnits = false;
      }
  }

}

</script>

