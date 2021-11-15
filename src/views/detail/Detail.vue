<template>
    <div id="detail">
        <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"/>

        
        <scroll class="content"
        :pull-up-load="true"
        :probe-type="3"
        @scroll="contentScroll"
        ref="scroll">
            <detail-swiper :top-images="topImages"/>
            <detail-base-info :goods="goods"/>
            <detail-shop-info :shop="shop"/>
            <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"/>
            <detail-param-info :param-info="paramInfo" ref="params"/>
            <detail-comment-info :comment-info="commentInfo" ref="comment"/>
            <goods-list :goods="recommends" ref="recommend"/>
        </scroll>

        <detail-bottom-bar class="detail-bottom" @addCart="addToCart"/>

        <back-top @click.native="backClick" v-show="isShowBackTop"/>

    </div>
</template>

<script>
import DetailNavBar from './childComps/DetailNavBar.vue'
import DetailSwiper from './childComps/DetailSwiper.vue'
import DetailBaseInfo from './childComps/DetailBaseInfo.vue'
import DetailShopInfo from './childComps/DetailShopInfo.vue'
import DetailGoodsInfo from './childComps/DetailGoodsInfo.vue'
import DetailParamInfo from './childComps/DetailParamInfo.vue'
import DetailCommentInfo from './childComps/DetailCommentInfo.vue'
import DetailBottomBar from './childComps/DetailBottomBar.vue'

import GoodsList from '@/components/content/goods/GoodsList'

import Scroll from '@/components/common/scroll/Scroll.vue'

import { getDetail , getRecommend , Goods , Shop , GoodsParam} from '@/network/detail'
import { backTopMixin } from '@/common/mixin'
export default {
    name:"Detail",
    components:{
        DetailNavBar,
        DetailSwiper,
        DetailBaseInfo,
        DetailShopInfo,
        DetailGoodsInfo,
        DetailParamInfo,
        DetailCommentInfo,
        DetailBottomBar,
        GoodsList,
        Scroll,
    }, 
    mixins:[backTopMixin],
    data(){
        return{
            iid : null,
            topImages:[],
            goods:{},
            shop:{},
            detailInfo:{},
            paramInfo:{},
            commentInfo:{},
            recommends:[],
            themeTopYs:[],
            currentIndex:0
        }
    },
    created(){
        this.iid=this.$route.params.iid

        getDetail(this.iid).then(res=>{
            // console.log(res);
            const data = res.result;    

            this.topImages = data.itemInfo.topImages;

            this.goods = new Goods(data.itemInfo,data.columns,data.shopInfo.services);

            this.shop = new Shop(data.shopInfo);

            this.detailInfo = data.detailInfo

            this.paramInfo = new GoodsParam(data.itemParams.info,data.itemParams.rule)

            if(data.rate.cRate !==0){
                this.commentInfo =data.rate.list[0]
            }

            
            // console.log(this.res);
        });
        
        getRecommend().then(res=>{
            // console.log(res.data.list);
            this.recommends=res.data.list;
        })
        
        
    },
    methods:{
            imageLoad(){
                this.$refs.scroll.scroll.refresh()
                setTimeout(() => {
                    this.$nextTick(()=>{
                    this.themeTopYs=[]

                    this.themeTopYs.push(0);  
                    this.themeTopYs.push(this.$refs.params.$el.offsetTop);
                    this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
                    this.themeTopYs.push(this.$refs.recommend.$el.offsetTop);
                    this.themeTopYs.push(Number.MAX_VALUE)
                    // console.log(this.themeTopYs);
                    })
                }, 100);
            },
            titleClick(index){
                // console.log(index);
                this.$refs.scroll.scroll.scrollTo(0,-this.themeTopYs[index],500);
                this.currentIndex=index
            },
            contentScroll(position){
                // console.log(position);
                const positionY = -position.y;

                let length = this.themeTopYs.length
                for(let i = 0; i < length-1; i++){
                    if(this.currentIndex !== i && 
                    (positionY >= this.themeTopYs[i] && positionY < this.themeTopYs[i+1])){
                        // console.log(i);
                        this.currentIndex = i;
                        this.$refs.nav.currentIndex = this.currentIndex
                    }
                }

                this.isShowBackTop = (-position.y) > 1000;
            },
            addToCart(){
                const product = {}
                product.image = this.topImages[0];
                // console.log(this.goods);
                product.title = this.goods.title;
                product.desc = this.goods.desc;
                product.price = this.goods.realPrice;
                product.iid = this.iid;

                this.$store.dispatch('addCart',product).then(res=>{
                    // console.log(res);
                    this.$toast.show(res,1000)
                    // console.log(this.$toast);
                })
            }
        }
    }
</script>

<style scoped>
 #detail{
     position: relative;
     z-index: 1001;
     background-color: #fff;
     height: 100vh;
 }
 .content{
     position: absolute;
     top: 44px;
     left: 0;
     right: 0;
     bottom: 49px;
     overflow: hidden;
 }
 .detail-nav{
     position: relative;
     z-index: 1001;
     background-color: #fff;
 }
</style>