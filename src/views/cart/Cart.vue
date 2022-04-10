<template>
    <div class="cart">
         <!--导航-->
         <nav-bar class="nav-bar">
             <div slot="center">购物车({{cartLength}})</div>
         </nav-bar>
         <!--商品的列表-->
         <cart-list/>
         <!--底部汇总-->
         <cart-bottom-bar/>
    </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar'
  import CartList from './childComps/CartList.vue'
  import CartBottomBar from './childComps/CartBottomBar.vue'

  import {mapGetters} from 'vuex'


export default {
    name:"Cart",
    components:{
        NavBar,
        CartList,
        CartBottomBar
    },
    computed:{
        ...mapGetters([
            'cartLength',
            'cartList'
        ])
    }
}
</script>
<style scoped>
   .nav-bar{
       background-color: var(--color-tint);
   }
</style>