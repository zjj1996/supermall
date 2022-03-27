<template>
    <tab-bar>
        <tar-bar-item path="/home">
          <img slot="item-icon" src="~assets/img/tabbar/home.svg" alt="">
          <img slot="item-icon-active" src="~assets/img/tabbar/home_active.svg" alt="">
          <div slot="item-text">首页</div>
        </tar-bar-item>
        <tar-bar-item path="/category">
          <img slot="item-icon" src="~assets/img/tabbar/category.svg" alt="">
          <img slot="item-icon-active" src="~assets/img/tabbar/category_active.svg" alt="">
          <div slot="item-text">分类</div>
        </tar-bar-item>
        <tar-bar-item path="/cart">
          <img slot="item-icon" src="~assets/img/tabbar/shopcart.svg" alt="">
          <img slot="item-icon-active" src="~assets/img/tabbar/shopcart_active.svg" alt="">
          <div slot="item-text">购物车</div>
        </tar-bar-item>
        <tar-bar-item path="/profile">
          <img slot="item-icon" src="~assets/img/tabbar/profile.svg" alt="">
          <img slot="item-icon-active" src="~assets/img/tabbar/profile_active.svg" alt="">
          <div slot="item-text">我的</div>
        </tar-bar-item>
     </tab-bar>
</template>

<script>
import TabBar from 'components/common/tabbar/TabBar'
import TarBarItem from 'components/common/tabbar/TarBarItem';


export default {
  name:"MainTabbar",
  components: {
    TabBar,
    TarBarItem
  },
}
</script>

<style scoped>
</style>