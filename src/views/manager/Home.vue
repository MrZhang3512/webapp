<template>
<!--  header-->
  <div class="home">
    <header class="header">
      <!-- 轮播图 -->
      <van-swipe class="my-swipe" :autoplay="3000" indicator-color="white" loop touchable>
        <van-swipe-item v-for="(i,index) in categories" :key="index">
          <img :src="i.icon"/>
        </van-swipe-item>
      </van-swipe>
      <!-- /轮播图 -->
    </header>
   <!-- content -->
   <!-- 分类6个 -->
    <van-grid :column-num="3">
    <van-grid-item
      v-for="value in categories"
      :key="value.id"
      :icon="value.icon"
      :text="value.name"
    />
  </van-grid>
   <!-- 产品4个 -->
    <briup-product-item
      v-for="p in products" 
      :key="p.id" 
      :data="p"
      @click="toBuyHandler(p)"
    >
  </briup-product-item>
  </div>
</template>
<script>
import {get,post} from '../../http/axios'
export default {
  data(){
    return{
      categories:[],
      products:[]
    }
  },
  created(){
    //查询栏目信息
    this.loadCategories();
    //查询产品
    this.loadProducts();
  },
  methods:{
    //加载栏目信息
    loadCategories(){
      let url="/category/findAll";
      get(url).then((response)=>{
        //将查询结果，数组中的前六个拿处理
        this.categories=response.data.slice(1,7);
      })
    },
    //加载产品信息
    loadProducts(){
      let url="/product/query"
      let params={
        page:0,
        pageSize:10
      }
      post(url,params).then((response)=>{
        this.products=response.data.list;
      })
    },
    toBuyHandler(p){
      //跳转到订单确认页面，并且携带数据p
      this.$router.push({
        path:"/manager/order_confirm",
        query:p
      })
    }

  }
}
</script>
<style scoped>
.home {
  padding-bottom: 50px;
}
.header {
  height: 250px;
  overflow: hidden;
}
.header img {
  width: 100%;
}
</style>