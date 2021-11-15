<template>  
    <div class="item-all">
        <div class="cart-list-item">
            <div class="check-box">
                <check-button :is-checked="product.checked"
                @click.native="CheckClick"/>
            </div>
            <div class="cart-left">
                <img :src="product.image" alt="商品图片">
            </div>
            <div class="cart-right">
                <div class="product-title">{{product.title}}</div>
                <div class="product-desc">{{product.desc}}</div>
                <div class="cart-bottom">
                    <span class="product-price">￥{{product.price}}</span>
                    <span>x{{product.count}}</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import CheckButton from '@/components/content/checkButton/checkButton.vue'
export default {
    name:"CartListItem",
    props:{
        product:{
            type:Object,
            default(){
                return{}
            }
        }
    },
    components:{
        CheckButton
    },
    methods:{
        CheckClick(){
            this.product.checked = !this.product.checked
        }
    }
}
</script>

<style scoped>
.cart-list-item{
    width: 100%;
    display: flex;
    border-bottom: 1px solid #ccc;

}
.check-box{
    /* width: 20px; */
    display: flex;
    justify-content: center;
    align-items: center;
    margin-left: 5px;
}
.cart-left{
    float: left;
    padding: 10px;
    overflow: hidden;
}
.cart-right{
    float: left;
    width: 68%;
    padding: 10px;
}
.cart-left img{
    width: 100%;
    border-radius: 5px;

}
.product-title,
.product-desc{
    overflow: hidden;
    text-overflow:ellipsis;
    white-space: nowrap;
}
.product-title{
    font-size: 18px;
    color: #000;
    padding-bottom: 20px;
}
.product-desc{
    font-size: 15px;
    padding-bottom: 30px;
}
.cart-bottom{
    display: flex;
    justify-content: space-between;
    padding-left: -2px;
    padding-right: 5px;
    font-size: 20px;
}
.product-price{
    color: orange;
}
</style>