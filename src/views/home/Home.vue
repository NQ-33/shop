<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>

    <tab-control :titles="['流行','新款','精选']"
      @tabClick="tabClick" ref="tabControlShow"
      v-show="isTabFixed" class="tabControl"/>

    <scroll class="content" 
    ref="scroll" 
    :probe-type="3" 
    @scroll="contentScroll"
    :pull-up-load="true"
    @pullingUp="loadMore">
    
      <home-swiper :banners="banners" @imgLoad="imgLoad"/>
      <recommend-view :recommends="recommends"/>
      <feature-view class="feature"/>
      <tab-control :titles="['流行','新款','精选']"
      @tabClick="tabClick" ref="tabControl"/>
      <goods-list :goods="showGoods"/>
    </scroll>

    <back-top @click.native="backClick" v-show="isShowBackTop"/>
  </div>
</template>

<script>
import HomeSwiper from './childComps/HomeSwiper.vue'
import FeatureView from './childComps/FeatureView.vue'
import RecommendView from '@/views/home/childComps/RecommendView.vue'

import TabControl from '@/components/content/tabControl/TabControl.vue'
import NavBar from '@/components/common/navbar/NavBar.vue'
import GoodsList from '@/components/content/goods/GoodsList'
import Scroll from '@/components/common/scroll/Scroll.vue'

import {getHomeMultidata,getHomeGoods} from '@/network/home.js'
import {backTopMixin} from '@/common/mixin.js'

export default {
    name: "Home",
    components: {
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl,
      NavBar,
      GoodsList,
      Scroll
      },
      data() {
        return{
          banners:[],
          recommends:[],
          goods:{
            'pop':{page:0, list:[]},
            'new':{page:0, list:[]},
            'sell':{page:0, list:[]},
          },
          currentType:'pop',
          tabOffsetTop:0,
          isTabFixed:false
        }
      },
      mixins:[backTopMixin],
      created(){
        this.getHomeMultidata()

        this.getHomeGoods('pop')
        this.getHomeGoods('new')
        this.getHomeGoods('sell')

      },
      computed:{
        showGoods(){
          return this.goods[this.currentType].list
        }
      },
      methods:{
        // 事件监听
        tabClick(index){
          switch (index){
            case 0:
              this.currentType='pop'
              break
            case 1:
              this.currentType='new'
              break
            case 2:
              this.currentType='sell'
              break
          }
          this.$refs.tabControl.currentIndex =index;
          this.$refs.tabControlShow.currentIndex=index;
        },
        contentScroll(position){
          this.isShowBackTop = (-position.y) > 1000;
          this.isTabFixed = (-position.y) >this.tabOffsetTop;
        },
        loadMore(){
          this.getHomeGoods(this.currentType)
        },
        imgLoad(){
          setTimeout(() => {
          this.tabOffsetTop=this.$refs.tabControl.$el.offsetTop;
          }, 100);
        },
        // 网络请求
        getHomeMultidata(){
          getHomeMultidata().then(res=>{
          // console.log(res);
            this.banners=res.data.banner.list;
            this.recommends=res.data.recommend.list;
          })
        },
        getHomeGoods(type){
          const page = this.goods[type].page + 1
          getHomeGoods(type, page).then(res => {
            this.goods[type].list.push(...res.data.list);
            this.goods[type].page += 1
            // console.log(res);
            this.$refs.scroll.scroll.finishPullUp()
          })
        }
      }
    }
</script>

<style scoped>
#home{
  height: 100%;
  position: relative;

}
.home-nav{
  background-color: var(--color-tint);
  color: aliceblue;
  /* position: fixed;
  left: 0;
  top: 0;
  right: 0;
  z-index: 999; */
}
.tabControl{
  position: relative;
  z-index: 999;
}
.content{
  position: absolute;
  top: 44px;
  left: 0;
  right: 0;
  bottom: 49px;
  overflow: hidden;
}
</style>
