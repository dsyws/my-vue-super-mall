<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <homeSwiper :banners="banners"></homeSwiper>
    <recommend-view :recommends="recommends"></recommend-view>
  </div>
</template>
<script>
import navBar from "components/common/navBar/NavBar";
import homeSwiper from "views/home/childrenComponents/HomeSwiper";
import recommendView from "views/home/childrenComponents/Recommend";

import { getHomeMultidata } from "network/home";
export default {
  name: "Home",
  data() {
    return { banners: null, recommends: null };
  },
  components: {
    navBar,
    homeSwiper,
    recommendView,
  },
  created() {
    getHomeMultidata().then((res) => {
      this.banners = res.data.banner.list;
      this.recommends = res.data.recommend.list;
      console.log(this.banner);
      console.log(this.recommend);
    });
  },
};
</script>
<style scoped>
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
}
</style>