<template>
    <div id="detail">
       <detail-nav-bar class="detail-nav"></detail-nav-bar>
       <scroll class="content" ref="scroll">
          <detail-swiper :topImages="topImages"></detail-swiper>
          <DetailBaseInfo :goods="goods"></DetailBaseInfo>
          <DetailShopInfo :shop="shop"></DetailShopInfo>
          <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"/>
          <detail-paramInfo :param-info="paramInfo"/>
       </scroll>
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

 import {getDetail,Goods,Shop,GoodsParam} from "network/detail"
   
export default {
  
   name:"Detail",
   components:{
       DetailNavBar,
       DetailSwiper,
       DetailBaseInfo,
       DetailShopInfo,
       DetailGoodsInfo,
       DetailParamInfo,
       DetailCommentInfo,
       Scroll
   },
   data(){
      return {
          iid:null,
          topImages:[],
          goods:{},
          shop:{},
          detailInfo:{},
          paramInfo:{},
          commentInfo:{}
      }
   },
   created(){
      //1、保存传入的iid
      this.iid=this.$route.params.iid

      //2、根据传入的iid请求详情数据
      getDetail(this.iid).then(res=>{
         console.log(res);
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
   },
   methods:{
      imageLoad(){
          this.$refs.scroll.refresh()
      }
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
      height: calc(100% - 44px);
   }
</style>