<template>
  <briup-fulllayout title="订单确认">
  服务地址：
    <van-dropdown-menu>
      <van-dropdown-item v-model="addressId" :options="options" />
    </van-dropdown-menu>
  订单详情：
    <div style="padding:0 2em">
        <p>服务名称：{{$route.query.name}}</p>
        <p>服务价格：{{$route.query.price}}</p>
        <p>服务数量：1</p>
        <p>服务小计：{{$route.query.price}}</p>
    </div>
    <div style="position:fixed;bottom:0;width:100%">
    <van-button block type="primary" @click="submit">提交按钮</van-button>
    </div>
  </briup-fulllayout>
</template>
<script>
import { mapState } from 'vuex'
import {get,post_obj_array} from '../../../http/axios'
export default {
  data(){
    return {
      addressId:0,    // 服务地址id
      addresses:[],
      orederLines:[]
    }
  },
  created(){
    // 在页面加载的时候查询地址信息
    this.loadAddress();
    //初始化订单向,将我们的产品放入到购物车中
    let orederLine={
        number:1,
        price:this.$route.query.price,
        productId:this.$route.query.id,
    };
    this.orederLines.push(orederLine);
  },
  computed:{
    // 映射info信息
    ...mapState("user",["info"]),
    // [{id:1,province:'山西省',city:"晋中市"},{id:1,province:'江苏省',city:"昆山市"}] =》 [{text:"山西省晋中市",value:1}]
    options:function(){
      // 将addresses的数据计算为一个新的数组返回
      return this.addresses.map(item=>{
        return {
          text:item.province+" "+item.city+" "+item.area+" "+item.address,
          value:item.id
        }
      })
    }
  },
  methods:{
    // 查询地址信息
    loadAddress(){
      let id = this.info.id;  
      let url = "/address/findByCustomerId?id="+id;
      get(url).then((response)=>{
        // 将查询到的地址信息绑定到address这个变量中
        this.addresses = response.data;
        // 将查询到的地址列表中第一个地址的id赋值给data中addressId,表示默认地址
        this.addressId = this.addresses[0].id;
      })
    },
    submit(){
        let url="/order/save"
        let data={
            customerId:this.info.id,
            addressId:this.addressId,
            orderLines:this.orederLines
        }
        post_obj_array(url,data).then((res)=>{
            this.$toast.success("下单成功");
           this.$router.push({
               path:'order'
           });
            
        })
    }
  }
}
</script>
