<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>
<script>
import Bscroll from "better-scroll";
export default {
  props: {
    probeType: {
      type: Number,
      default() {
        return;
      },
    },
    pullUpLoad: {
      type: Boolean,
      default() {
        return false;
      },
    },
  },
  data() {
    return {
      scroll: null,
    };
  },
  mounted() {
    this.scroll = new Bscroll(this.$refs.wrapper, {
      click: true,
      probeType: this.probeType,
      pullUpLoad: this.pullUpLoad,
    });
    if (this.probeType === 2 || this.probeType === 3) {
      this.scroll.on("scroll", (position) => {
        this.$emit("scroll", position);
      });
    }
    if (this.pullUpLoad) {
      this.scroll.on("pullingUp", () => {
        this.$emit("pullingUp");
      });
    }
  },
  methods: {
    scrollT(x, y, time = 300) {
      this.scroll.scrollTo(x, y, time);
    },
    scrollRefresh() {
      this.scroll.refresh();
    },
    getScrollY(){
      return this.scroll.y
    }
  },
};
</script>
<style scoped>
/* .wrapper {
  height: 100px;
  background-color: green;
  color: #fff;
  overflow: hidden;
} */
</style>