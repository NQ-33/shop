<template>
  <div>
      <div class="top-content">
        <div v-for="(item, index) in rightList" :key="index" class="top-item">
          <a :href="item.link">
            <img :src="item.image" alt="" />
            <p>{{ item.title }}</p>
          </a>
        </div>
      </div>
  </div>
</template>

<script>

export default {
  name: "CategoryRight",
  data() {
    return {
      currentIndex:0,
      currentType:'pop',
    };
  },
  props: {
    rightList: {
      type: Array,
      default() {
        return [];
      },
    }
  },
};
</script>

<style scoped>
.top-content {
  width: 100%;
}
.top-item {
  display: inline-block;
  width: 33%;
  padding: 10px;
}
.top-item a img {
  width: 100%;
}
.top-item a p {
  text-align: center;
}
</style>