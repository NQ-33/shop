<template>
  <div v-show="isshow">
    <div class="toast">{{ message }}</div>
  </div>
</template>

<script>
export default {
  name: "Toast",
  //   props: {
  //     message: {
  //       type: String,
  //       default: "",
  //     },
  //     show:{
  //         type:Boolean,
  //         default:false
  //     }
  //   },
  data() {
    return {
      message: "",
      isshow: false,
    };
  },
  methods: {
    show(message, duration) {
      this.isshow = true;
      this.message = message;

      setTimeout(() => {
        this.isshow = false;
        this.message = "";
      }, duration);
    },
  },
};
</script>

<style scoped>
.toast {
  position: fixed;
  top: 50%;
  left: 50%;
  padding: 8px 10px;
  transform: translate(-50%,-50%);
  color: white;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 9999;
  border-radius: 8px;
}
</style>