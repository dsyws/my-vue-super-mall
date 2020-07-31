<template>
  <div id="swiper">
    <swiper>
      <swiper-item v-for="item in banner">
        <a :href="item.link">
          <img :src="item.image" />
        </a>
      </swiper-item>
    </swiper>
  </div>
</template>
<script>
import { Swiper, SwiperItem } from "components/common/swiper";
export default {
    props:{
        banner:{
            type:Array,
            default(){
                return []
            }
        }
    },
  components: {
    Swiper,
    SwiperItem
  },
};
</script>
<style scoped>
</style>