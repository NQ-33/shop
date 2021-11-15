<template>
  <div class="shop-info">
      <div class="shop-info-title">
          <div class="shop-logo"><img :src="shop.logo" alt=""></div>
          <div class="shop-name">{{shop.name}}</div>
      </div>

      <div class="sell-value">
          <div class="value-left">
              <div>
                  <div class="sells">{{towNumber(shop.sells/1000)}}万</div>
                  <div class="sells-text">总销量</div>
              </div>
              <div>
                  <div class="goods-all">{{shop.goodsCount}}</div>
                  <div class="goods-all-text">全部宝贝</div>
              </div>
          </div>

          <div class="value-right">
              <div v-for="(item,index) in shop.score" :key="index">
                  <div class="score-all">
                      <div class="score-name">{{item.name}}</div>
                      <div class="item-flex score-false" :class="{'score-ture':item.isBetter}">{{item.score}}</div>
                      <div class="item-flex">
                          <span  class="better-false" :class="{'better-ture':item.isBetter}">{{item.isBetter ? '高':'低'}}</span>
                      </div>
                  </div>
              </div>
          </div>
      </div>

      <div class="shop-bottom">
          <div class="enter-shop">进店逛逛</div>
      </div>
  </div>
</template>

<script>
export default {
    name:"DetailShopInfo",
    props:{
        shop:{
            type:Object,
            default(){
                return{}
            }
        }
    },
    methods:{
        towNumber(val) {      
            return val.toFixed(1)    
        }
    }
}
</script>

<style scoped>
.shop-info-title{
    display: flex;
    margin: 20px 10px;
}
.shop-logo img{
    border-radius: 25px;
    width: 50px;
    height: 50px;
    
}
.shop-name{
    font-size: 18px;
    color: #000;
    height: 50px;
    line-height: 50px;
    padding-left: 20px;
}


.sell-value{
    display: flex;
    justify-content: space-around;
}
.value-left{
    display: flex;
    width: 50%;
    justify-content: space-evenly;
    margin: 20px 0;
    border-right: 1px solid #ccc;
}
.sells,
.goods-all{
    font-size: 22px;
    text-align: center;
    padding-bottom: 7px;
}
.sells-text,
.goods-all-text{
    font-size: 14px;
    text-align: center;

}
.value-right{
    width: 50%;
    font-size: 14px;
    padding-left: 30px;
}
.score-all{
    margin: 10px;
    display: flex;   
}
.score-name{
    flex: 2;
}
.item-flex{
    flex: 1;
}
.score-false{
    color: green;
}
.better-false{
    color: white;
    background-color: green;
}
.score-ture{
    color: red;
}
.better-ture{
    color: white;
    background-color: red;
    width: 16px;
}
.shop-bottom{
    margin: 5px 0;
}
.enter-shop{
    text-align: center;
    font-size: 20px;
}
</style>