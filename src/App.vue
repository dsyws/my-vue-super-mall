<template>
  <div id="app">
    <mainTabBar></mainTabBar>
    <router-view></router-view>
  </div>
</template>

<script>
import mainTabBar from 'components/content/mainTabBar/MainTabBar'
export default {
  name: "App",
  components: {
    mainTabBar
  }
};
</script>

<style>
@import './assets/css/base.css'
</style>
