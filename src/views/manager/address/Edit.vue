<template>
    <div>
         <briup-fulllayout title="添加地址">
         <div>
            <!-- 标签 -->
            <div class="myTag">
            <van-tag type="primary" size="large">{{info.name}}</van-tag>
            <!-- /标签 -->
            </div>
            <van-form @submit="submitHandler">
                <van-field
                    v-model="form.telephone"
                    name="联系方式"
                    label="联系方式"
                    placeholder="联系方式"
                    :rules="[{ required: true, message: '请填写联系方式' }]"
                />
                <van-field
                    v-model="form.province"
                    name="省份"
                    label="省份"
                    placeholder="省份"
                    :rules="[{ required: true, message: '请填写省份' }]"
                />
                <van-field
                    v-model="form.city"
                    name="市"
                    label="市"
                    placeholder="市"
                    :rules="[{ required: true, message: '请填写市' }]"
                />
                <van-field
                    v-model="form.area"
                    name="县"
                    label="县或区或市"
                    placeholder="县或区"
                    :rules="[{ required: true, message: '请填写县或区或市' }]"
                />
                <van-field
                    v-model="form.address"
                    name="详细地址"
                    label="详细地址"
                    placeholder="详细地址"
                    :rules="[{ required: true, message: '请填写详细地址' }]"
                />
                <div style="margin: 16px;">
                    <van-button round block type="info" native-type="submit">
                    提交
                    </van-button>
                    <van-button round block type="warning" @click="deleteHandler">
                    删除
                    </van-button>
                </div>
                </van-form>
         </div>
         </briup-fulllayout>
    </div>
</template>
<script>
import {mapState} from 'vuex'
import { post,response,get } from '../../../http/axios';
// import area from '../../../plugins/area'
export default {
    
    data(){
        return{
            form:{},
            
        }
    },
    computed:{
        ...mapState('user',['info']),
        //当前地址信息
        customerAddress(){
            return this.$route.query.address;
        }
    },
    created(){
    //   console.log(this.$route.query.address)
        if(this.customerAddress.id){
        this.form.id = this.customerAddress.id;  
        }
        this.form.telephone = this.customerAddress.tel;  
        this.form.province = this.customerAddress.province; 
        this.form.city = this.customerAddress.city;  
        this.form.area = this.customerAddress.area;
        this.form.address = this.customerAddress.addressDetail;  



    },
    methods:{
        submitHandler(){
            let url="/address/saveOrUpdate";
            //在提交前，将当前登录者的ID作为顾客ID
            this.form.customerId=this.info.id;
            post(url,this.form).then((res)=>{
                 //返回上一个页面
                this.$router.go(-1);
                this.$toast.success(res.statusText);
            })
           
        },
        //删除按钮
        deleteHandler(){
            get('address/deleteById',{id:this.customerAddress.id})
            .then(res=>{
                this.$router.go(-1); 
                this.$toast.success(res.statusText); 
            })
        }
    }    
}
</script>
<style scoped>
.myTag {
    text-align: center;
}
</style>