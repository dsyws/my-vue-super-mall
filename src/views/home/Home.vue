<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
   <homeSwiper :banner="banner"></homeSwiper>
  </div>
</template>
<script>
import navBar from "components/common/navBar/NavBar";
import { getHomeMultidata } from "network/home";
import homeSwiper from 'views/home/childrenComponents/HomeSwiper'
export default {
  name: "Home",
  data() {
    return { banner: null, recommend: null };
  },
  components: {
    navBar,
    homeSwiper
  },
  created() {
    getHomeMultidata().then((res) => {
      this.banner = res.data.banner.list;
      this.recommend = res.data.recommend.list;
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