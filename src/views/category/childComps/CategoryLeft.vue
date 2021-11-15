<template>
  <div class="left-all">
    <scroll class="content"
    :pull-up-load="true">
      <ul>
        <li v-for="(item, index) in leftList" :key="index"
          class="list-item"
          :class="{ active: index === LeftCurrentIndex }"
          @click="itemClick(index)">
          {{ item.title }}
        </li>
      </ul>
    </scroll>
  </div>
</template>

<script>
import Scroll from "@/components/common/scroll/Scroll.vue";
export default {
  name: "CategoryLeft",
  data() {
    return {
      LeftCurrentIndex: 0,
      maitKey:''
    }
  },
  components: {
    Scroll,
  },
  props: {
    leftList: {
      type: Array,
      default() {
        return [];
      },
    }
  },
  methods: {
    itemClick(index) {
      this.LeftCurrentIndex = index;
    
      this.$emit('leftClick',index)
    },
  },
};
</script>

<style scoped>
.content {
  width: 120px;
  position: absolute;
  top: 44px;
  left: 0;
  right: 0;
  bottom: 49px;
  overflow: hidden;
  background-color: gainsboro;
}
.list-item {
  box-sizing: border-box;

  padding: 10px 18px;
  text-align: center;
  color: rgb(100, 100, 100);
  font-size: 18px;
}
.active {
  color: var(--color-high-text);
  border-left: 3px solid var(--color-high-text);
}
</style>