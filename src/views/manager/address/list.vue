<template>
    <div>
        <briup-fulllayout title="地址管理">
        <!-- 地址列表 -->
        <van-address-list
            v-model="chosenAddressId"
            :list="addresses"
            :switchable="false"
            @add="toAddressEditHander"
            @edit="toEdit"
            @select="toSelect"
            />
        <!-- /地址列表 -->
        </briup-fulllayout>
    </div>
</template>
<script>
import {get,response} from '../../../http/axios'
import {mapState} from 'vuex'
export default {
    data(){
        return{
            addresses: [],
            //地址默认id 
            chosenAddressId:"",
        }
    },
    methods:{
    //加载地址信息
    loadAddresses(){
      let id=this.info.id;
      let url="/address/findByCustomerId?id="+id;
      get(url).then((response)=>{
        response.data.forEach(item=>{
            //将返回的地址信息进行处理
            item.id = item.id;
            item.name = this.info.name;
            item.tel = item.telephone;
            item.addressDetail = item.address;
            item.address = item.province+item.city+item.area+item.address;
        });
        this.addresses = response.data;
        //将第一个地址设置为默认地址
        this.chosenAddressId = this.addresses[0].id;
      })
    },
    //添加地址
    toAddressEditHander(){
        this.$router.push("/manager/address_edit")
    },
    //编辑地址
    toEdit(item){
        //通过路由传参将当前行的信息传递
        this.$router.push({path:"/manager/address_edit",query:{address:item}})
    },
    //切换地址
    toSelect(item,index){
        this.chosenAddressId = item.id;
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