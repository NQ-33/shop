<template>
    <div class="check-button" :class="{check: isChecked}">
        <img src="@/assets/img/cart/tick.svg" alt="">
    </div>
</template>

<script>
export default {
    name:"CheckButton",
    props:{
        isChecked:{
            type:Boolean,   
            default:true
        }
    }
}
</script>

<style scoped>
.check-button{
    /* background-color: red; */
    height: 19px;
    width: 19px;
    border-radius: 50%;
    border: 1px solid #aaa;
}
.check{
    border-color: red;
    background-color: red;
}
</style>