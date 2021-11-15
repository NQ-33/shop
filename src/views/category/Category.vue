<template>
  <div id="category">
    <category-nav-bar class="g-nav-bar">
      <div slot="center">商品分类</div>
    </category-nav-bar>

    <div class="content-all">
      <category-left
        :left-list="leftList"
        @leftClick="leftClick"
        @click.native="fastBackClick"/>
      <div class="right">
        <tab-control :titles="['综合', '新品', '销量']"
          @tabClick="tabClick" ref="tabControlShow"
          class="tabControl" v-show="isTabFixed"/>

        <scroll ref="scroll" 
        class="content" 
        :pull-up-load="true"
        :probe-type="3"
        @scroll="contentScroll">

          <category-right :right-list="rightList"/>

          <tab-control
            ref="tabControl"
            :titles="['综合', '新品', '销量']"
            @tabClick="tabClick"/>

          <goods-list :goods="showGoods"/>

        </scroll>
      </div>
    </div>
  </div>
</template>

<script>
import CategoryNavBar from "@/components/common/navbar/NavBar.vue";
import Scroll from "@/components/common/scroll/Scroll.vue";

import TabControl from "@/components/content/tabControl/TabControl.vue";
import GoodsList from '@/components/content/goods/GoodsList.vue';


import CategoryLeft from "./childComps/CategoryLeft.vue";
import CategoryRight from "./childComps/CategoryRight.vue";

import {
  getCategory,
  getSubCategory,
  getSubCategoryDetail,
} from "@/network/category";

import {backTopMixin} from '@/common/mixin.js'

export default {
  name: "Category",
  components: {
    CategoryNavBar,
    Scroll,
    TabControl,
    GoodsList,
    CategoryLeft,
    CategoryRight,
  },
  mixins:[backTopMixin],

  data() {
    return {
      leftList: [],
      LeftCurrentIndex: 0,
      currentType: "pop",
      rightList: [],
      goods: {
        pop: [],
        new: [],
        sell: [],
      },
      isTabFixed:false
    };
  },
  computed:{
    showGoods(){
          return this.goods[this.currentType]
        }
  },
  methods: {
    getRightTop() {
      getSubCategory(this.leftList[this.LeftCurrentIndex].maitKey).then((res) => {
        this.rightList = res.data.list;
      });
    },
    getRightBottom(type) {
      getSubCategoryDetail(
        this.leftList[this.LeftCurrentIndex].miniWallkey,
        type
      ).then((res) => {
        // console.log(res);
        this.goods[type] = res;
        // console.log(this.goods);
      });
    },
    getRightBottomAll() {
      this.getRightBottom("pop");
      this.getRightBottom("new");
      this.getRightBottom("sell");
    },
    getCategoryAll() {
      getCategory().then((res) => {
        // console.log(res);
        this.leftList = res.data.category.list;
      });
      setTimeout(() => {
        this.getRightTop();
        this.getRightBottomAll();
      }, 200);

      // setTimeout(() => {
      //   for (let item of this.leftList) {
      //     getSubCategory(item.maitKey).then((res) => {
      //         // console.log(res);
      //     this.rightList.push(res.data)

      //       });
      //   }
      // }, 100);
    },

    contentScroll(position){
      setTimeout(() => {
        this.tabOffsetTop=this.$refs.tabControl.$el.offsetTop;
      }, 100);
      this.isTabFixed = (-position.y) >this.tabOffsetTop;
    },
    reShow(){
      return this.goods[this.currentType]
    },
    leftClick(index) {
      this.LeftCurrentIndex = index;
      this.getRightTop();
      this.getRightBottomAll();
      this.$refs.tabControl.currentIndex =  0;
      this.$refs.tabControlShow.currentIndex = 0;
      this.currentType='pop'
      this.reShow()
    },
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
  },
  created() {
    this.getCategoryAll();
  },
};
</script>



<style scoped>
#category {
  height: 100%;
}
.content {
  position: absolute;
  top: 44px;
  left: 120px;
  right: 0;
  bottom: 49px;
  overflow: hidden;
  background-color: #fff;
}
.g-nav-bar {
  background-color: #ff8198;
  color: aliceblue;
}
.content-all {
  display: flex;
}
.tabControl{
  position: absolute;
  left: 120px;
  right: 0;
  z-index: 999;
}
</style>
