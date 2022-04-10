<template>
    <div id="detail">
       <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"></detail-nav-bar>
       <scroll class="content" ref="scroll" :probe-type="3" @scroll="contentScroll">
          <detail-swiper :topImages="topImages"></detail-swiper>
          <DetailBaseInfo :goods="goods"></DetailBaseInfo>
          <DetailShopInfo :shop="shop"></DetailShopInfo>
          <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"/>
          <detail-paramInfo :param-info="paramInfo" ref="params"/>
          <detail-comment-info :comment-info="commentInfo" ref="comment"/>
          <goods-list :goods="recommend" ref="recommend"/>
       </scroll>
       <detail-bottom-bar @addToCart="addToCart"/>
      <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
    </div>
</template>  

<script>
 import DetailNavBar from "views/detail/childComps/DetailNavBar"
 import DetailSwiper from "views/detail/childComps/DetailSwiper"
 import DetailBaseInfo from "views/detail/childComps/DetailBaseInfo"
 import DetailShopInfo from "views/detail/childComps/DetailShopInfo"
 import Scroll from "components/common/scroll/Scroll"
 import DetailGoodsInfo from "views/detail/childComps/DetailGoodsInfo"
 import DetailParamInfo from "views/detail/childComps/DetailParamInfo"
 import DetailCommentInfo from "views/detail/childComps/DetailCommentInfo"
 import GoodsList from "components/content/goods/GoodsList"
 import DetailBottomBar from "views/detail/childComps/DetailBottomBar"


 import {getDetail,Goods,Shop,GoodsParam,getRecommend} from "network/detail"
 import {debounce} from "common/utils"
 import {itemListenerMixin,backTopMixin} from 'common/mixin'
   
export default {
  
   name:"Detail",
   mixins:[itemListenerMixin,backTopMixin],
   components:{
       DetailNavBar,
       DetailSwiper,
       DetailBaseInfo,
       DetailShopInfo,
       DetailGoodsInfo,
       DetailParamInfo,
       DetailCommentInfo,
       Scroll,
       GoodsList,
       DetailBottomBar,
   },
   data(){
      return {
          iid:null,
          topImages:[],
          goods:{},
          shop:{},
          detailInfo:{},
          paramInfo:{},
          commentInfo:{},
          recommend:[],
          itemImgListener:null,
          themeTopYs:[],
          getThemeTopY:null,
          currentIndex:0,
          isShowBackTop:false
      }
   },
   created(){
      //1、保存传入的iid
      this.iid=this.$route.params.iid

      //2、根据传入的iid请求详情数据
      getDetail(this.iid).then(res=>{
         // console.log(res);
         const data=res.result;
         //1、获取顶部的图片轮播数据
         this.topImages=res.result.itemInfo.topImages

        //2、获取商品信息
        this.goods=new Goods(data.itemInfo,data.columns,data.shopInfo.services)

        //3、创建店铺信息的对象
        this.shop=new Shop(data.shopInfo)

        //4、获取下面图片展示数据
        this.detailInfo=data.detailInfo
       
       //5、获取参数的信息
       this.paramInfo=new GoodsParam(data.itemParams.info,data.itemParams.rule)

       //6、获取评论的信息
       if(data.rate!==0){
          this.commentInfo=data.rate.list[0]
       }

      })

      //3、获取推荐的详情数据
      getRecommend().then(res=>{
         // console.log(res)
         this.recommend=res.data.list
      })

      //4、给getThemeTopY赋值
      this.getThemeTopY=debounce(()=>{
         this.themeTopYs=[]
         this.themeTopYs.push(0)
         this.themeTopYs.push(this.$refs.params.$el.offsetTop-44)
         this.themeTopYs.push(this.$refs.comment.$el.offsetTop-44)
         this.themeTopYs.push(this.$refs.recommend.$el.offsetTop-44)

         // console.log(this.themeTopYs)
      },100)
 
   },
   methods:{
      imageLoad(){
          this.$refs.scroll.refresh()
          this.getThemeTopY()
      },
      titleClick(index){
          this.$refs.scroll.scrollTo(0,-this.themeTopYs[index],200)
      },
      contentScroll(position){
         //1、获取y值
         const positionY=-position.y
         
         let length=this.themeTopYs.length
         
         for(let i=0;i<length;i++){
            if(this.currentIndex!==i&&((i<length-1&&positionY>=this.themeTopYs[i]&&positionY<=this.themeTopYs[i+1])||(i===length-1&&positionY>=this.themeTopYs[i]))){
               this.currentIndex=i;
               this.$refs.nav.currentIndex=this.currentIndex
            }
         }
         
        //判断BackTop是否显示
         this.isShowBackTop=(-position.y)>1000

      },
      addToCart(){
           //获取购物车需要展示的信息
           const product={}
           product.image=this.topImages[0]
           product.title=this.goods.title
           product.desc=this.goods.desc
           product.price=this.goods.realPrice
           product.iid=this.iid
        
           //将商品添加到购物车
         //   this.$store.commit('addCart',product)
           this.$store.dispatch('addCart',product).then(res=>{
              this.$toast.show(res,2000)
           })
      } 

   },

   mounted(){
        
   },
   destroyed(){
       this.$bus.$off('itemImageLoad',this.itemImgListener)
   }
}
</script>
<style scoped>
   #detail{
      position: relative;
      z-index: 9;
      background-color: #fff;
      height: 100vh;
   }

   .detail-nav{
      position: relative;
      z-index: 9;
      background-color: #fff;
   }

   .content{
      height: calc(100% - 44px - 49px);
   }
</style>