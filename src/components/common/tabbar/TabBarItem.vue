<template>
  <div class="tab-bar-item" @click="itemClick">
    <div v-if="! isActive">
      <slot name="item-icon"></slot>
    </div>
    <div v-else>
      <slot name="item-icon-active"></slot>
    </div>
    <div :style="activeStyle">
      <slot name="item-text"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "tabBarItem",
  props: {
    path: String,
    activeColor: {
      type: String,
      default: "red"
    }
  },
  data() {
    return {
      //  isActive: false
    };
  },
  computed: {
    isActive() {
      return this.$route.path.indexOf(this.path) !== -1;
    },
    activeStyle() {
      return this.isActive ? { color: this.activeColor } : {};
    }
  },
  components: {},
  methods: {
    itemClick() {
      this.$router.replace(this.path);
    }
  }
};
</script>

<style>
.tab-bar-item {
  flex: 1;
  text-align: center;
  height: 49px;
  font-size: 14px;
  color:#1afa29;
}
.tab-bar-item img {
  margin-top: 3px;
  width: 20px;
  height: 20px;
  vertical-align: middle;
  margin-bottom: 1px;
}
</style>
