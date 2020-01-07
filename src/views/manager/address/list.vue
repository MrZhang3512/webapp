<template>
    <div>
        <briup-fulllayout title="地址管理">
<span>用户名:{{this.info.name}}</span>
<br>
        <van-list>
        <van-cell
            v-for="item in addresses"
            :key="item.id"
            :title="item.province+' '+item.city+' '+item.area+' '+item.address"
        />
        </van-list>
        <br>
        <van-button block type="default" @click="toAddressEditHander">添加地址</van-button>
        </briup-fulllayout>
    </div>
</template>
<script>
import {get,response} from '../../../http/axios'
import {mapState} from 'vuex'
export default {
    data(){
        return{
      addresses: []
        }
    },
    methods:{
    //加载地址信息
    loadAddresses(){
      let id=this.info.id;
      let url="/address/findByCustomerId?id="+id;
      get(url).then((response)=>{
        //将查询结果，数组中的前六个拿处理
        this.addresses=response.data;
      })
    },
    toAddressEditHander(){
        this.$router.push("/manager/address_edit")
    }
},
created(){
    this.loadAddresses();
},
computed:{//计算属性
    //将状态机中的user对象中的info对象获取到
    ...mapState("user",["info"])
}
}
</script>
<style scoped>

</style>