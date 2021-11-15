<template>
  <div class="cart-bottom-bar">
    <div class="btm-left">
      <check-button
        class="btm-bar-check"
        @click.native="checkClick"
        :is-checked="isSelectAll"
      />
      <span class="check-all">全选</span>
    </div>
    <div class="btm-center">
      <span>合计：￥{{ totalPrice }}</span>
    </div>
    <div class="btm-right" @click="checkGoods">去结算({{ checkedLength }})</div>
  </div>
</template>

<script>
import CheckButton from "@/components/content/checkButton/checkButton.vue";

import { mapGetters } from "vuex";

export default {
  name: "CartBottomBar",
  components: {
    CheckButton,
  },
  computed: {
    ...mapGetters(["cartList"]),
    totalPrice() {
      return this.$store.state.cartList
        .filter((item) => {
          return item.checked;
        })
        .reduce((preValue, item) => {
          return preValue + item.price * item.count;
        }, 0).toFixed(2);
    },
    checkedLength() {
      return this.cartList.filter((item) => item.checked).length;
    },
    isSelectAll() {
      if (this.cartList.length == 0) return false;
      //   return !(this.cartList.filter((item) => !item.checked).length)
      return !this.cartList.find((item) => !item.checked);
      //   for(let item of this.cartList){
      //       if(!item.checked){
      //           return false
      //       }
      //       return true
      //   }
    },
  },
  methods: {
    checkClick() {
        let trueLength = this.cartList.filter((item=>item.checked)).length
        if(trueLength == this.cartList.length){
            for(let item of this.cartList){
                item.checked = false
            }
        }else{
            for(let item of this.cartList){
                item.checked = true
            }
        }
        // if(this.isSelectAll){
        //     this.cartList.forEach(item =>item.checked=false);
        // }else{
        //     this.cartList.forEach(item =>item.checked=true);
        // }
    },
    checkGoods(){
      let lastLength =this.cartList.filter(item=>item.checked).length
      // console.log(lastLength);
      if(lastLength == 0){
      this.$toast.show('请选择商品',2000)

      }
    }
  },
};
</script>

<style scoped>
.cart-bottom-bar {
  height: 40px;
  line-height: 40px;

  display: flex;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 49px;

  background-color: #fff;
  border-top: 1px solid #ccc;
}
.check-all {
  font-size: 18px;
  margin-left: 5px;
}
.btm-center {
  font-size: 18px;
  color: #000;
  padding-left: 18px;
}
.btm-left {
  display: flex;
  align-items: center;
  margin-left: 10px;
}
.btm-right {
  background-color: orange;
  padding: 0 20px;
  position: absolute;
  right: 0;
  color: #fff;
  font-size: 18px;
}
.btm-bar-check {
  height: 20px;
  width: 20px;
  line-height: 0;
}
</style>