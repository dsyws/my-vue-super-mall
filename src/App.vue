<template>
  <div id="app">
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
    <mainTabBar></mainTabBar>
  </div>
</template>

<script>
import mainTabBar from "components/content/mainTabBar/MainTabBar";

export default {
  name: "App",
  components: {
    mainTabBar,
  },
};
</script>

<style>
@import "./assets/css/base.css";
</style>
