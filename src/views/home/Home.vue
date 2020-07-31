<template>
<div id="home">
  <nav-bar class="home-nav">
    <div slot="center">购物街</div>
  </nav-bar>
</div>
</template>
<script>
import navBar from "components/common/navBar/NavBar";
export default {
  components: {
    navBar
  },
};
</script>
<style scoped>
.home-nav{
  background-color: var(--color-tint);
  color: #fff;
}
</style>