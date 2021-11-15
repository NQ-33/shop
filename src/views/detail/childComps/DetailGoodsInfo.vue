<template>
    <div v-if="Object.keys(detailInfo).length !==0" class="goods-info">
        <div class="info-desc clear-fix">
            <div class="start"></div>
            <div>{{detailInfo.desc}}</div>
            <div class="end"></div>
        </div>
        <div class="info-key">{{detailInfo.detailImage[0].key}}</div>
        <div class="info-list">
            <img v-for="(item,index) in detailInfo.detailImage[0].list" 
            :src="item" alt="" :key="index" @load="imgLoad">
        </div>

    </div>
</template>

<script>
export default {
    name:"DetailGoodsInfo",
    props:{
        detailInfo:{
            type:Object,
            default(){
                return{}
            }
        }
    },
    data(){
        return{
            counter:0,
            imagesLength:0
        }
    },
    methods:{
        imgLoad(){
            if(++this.counter == this.imagesLength){
                this.$emit('imageLoad')
            }
        }
    },
    watch:{
        detailInfo(){
            this.imagesLength = this.detailInfo.detailImage[0].list.length
        }
    }
}
</script>

<style scoped>
.info-desc{
    margin: 10px;
    padding: 10px;
    line-height: 25px;
}
.info-key{
    padding-left: 20px;
    padding-bottom: 20px;
}
.start,
.end{
    border-top: 2px solid #ccc;
}
.info-list img{
    width: 100%;
}
</style>