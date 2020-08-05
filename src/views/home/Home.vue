<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <tabControl
      :titles="title"
      @tabClick="tabClicked"
      class="top_tab_control"
      v-show="isTabfixed"
      ref="tabControlTop"
    />
    <scroll
      class="content"
      ref="scroll"
      :probeType="3"
      @scroll="contentScroll"
      :pullUpLoad="pullUpLoad"
      @pullingUp="loadMore"
    >
      <homeSwiper :banners="banners" @swiperImageLoad="swiperImageLoad"></homeSwiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature></feature>
      <tabControl :titles="title" @tabClick="tabClicked" ref="tabControl" />
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <back-top @click.native="topClick" v-show="isShowBackTop"></back-top>
  </div>
</template>
<script>
import navBar from "components/common/navBar/NavBar";
import scroll from "components/common/scroll/Scroll";

import tabControl from "components/content/tabControl/TabControl";
import goodsList from "components/content/goods/GoodsList";
import backTop from "components/content/backTop/BackTop";

import homeSwiper from "views/home/childrenComponents/HomeSwiper";
import recommendView from "views/home/childrenComponents/Recommend";
import feature from "views/home/childrenComponents/Feature";

import { getHomeMultidata, getHomeGoods } from "network/home";
import { debounce } from "@/common/util";

export default {
  name: "Home",
  data() {
    return {
      banners: null,
      recommends: null,
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
      currentType: "pop",
      pullUpLoad: true,
      isShowBackTop: false,
      title: ["流行", "新款", "精选"],
      offsetTop: 528,
      isTabfixed: false,
      scrollY: 0,
    };
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list;
    },
  },
  activated() {
    console.log(this.scrollY);
    this.$refs.scroll.scrollRefresh();
    this.$refs.scroll.scrollT(0,this.scrollY, 0);
    
  },
  deactivated() {
    this.scrollY = this.$refs.scroll.getScrollY();
  },
  components: {
    navBar,
    homeSwiper,
    recommendView,
    feature,
    tabControl,
    goodsList,
    scroll,
    backTop,
  },
  created() {
    this.getHomeMultidata(), this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
  },
  mounted() {
    const refresh = debounce(this.$refs.scroll.scrollRefresh, 200);
    this.$bus.$on("itemImageLoad", () => {
      refresh();
    });
  },
  methods: {
    /**
     * emit
     */
    swiperImageLoad() {
      this.offsetTop = this.$refs.tabControl.$el.offsetTop;
    },
    loadMore() {
      this.getHomeGoods(this.currentType);
    },
    /**
     * click
     */

    contentScroll(position) {
      if (-position.y > this.offsetTop) {
        this.isTabfixed = true;
      } else {
        this.isTabfixed = false;
      }
      if (-position.y > 1000) {
        this.isShowBackTop = true;
      } else {
        this.isShowBackTop = false;
      }
    },
    topClick() {
      this.$refs.scroll && this.$refs.scroll.scrollT(0, 0);
    },
    tabClicked(index) {
      switch (index) {
        case 0:
          this.currentType = "pop";
          break;
        case 1:
          this.currentType = "new";
          break;
        case 2:
          this.currentType = "sell";
          break;
        default:
          this.currentType = "pop";
      }
      this.$refs.tabControlTop.currentIndex = index;
      this.$refs.tabControl.currentIndex = index;
    },
    /**
     * axios
     */
    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      let page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
        this.$refs.scroll.scroll.finishPullUp();
      });
    },
  },
};
</script>
<style scoped>
#home {
  height: 100vh;
  position: relative;
}
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
}
.content {
  overflow: hidden;
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}
.top_tab_control {
  position: relative;
  z-index: 99;
}
</style>