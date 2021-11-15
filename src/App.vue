<template>
  <div id="app" v-cloak>
    <keep-alive exclude="Detail,cart">
      <router-view/>
    </keep-alive>
    
    <main-tab-bar/>
  </div>
</template>
<script>
  import MainTabBar from '@/components/content/mainTabbar/MainTabBar.vue'
export default {
    name: 'app',
    components: {
      MainTabBar
    }
}
</script>
<style>
@import "./assets/css/base.css";
#app{
  height: 100vh;
}
[v-cloak]{
  display: none;
}
</style>
