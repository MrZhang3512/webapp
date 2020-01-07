<template>
    <div>
         <briup-fulllayout title="添加地址">
         <div>
             <van-cell-group>
                <van-field v-model="form.telephone" placeholder="手机号" />
             </van-cell-group>
              <br>
             <van-cell-group>
                <van-field v-model="form.province" placeholder="省" />
             </van-cell-group>
              <br>
             <van-cell-group>
                <van-field v-model="form.city" placeholder="市" />
             </van-cell-group>
              <br>
             <van-cell-group>
                <van-field v-model="form.area" placeholder="区" />
             </van-cell-group>
             <br>
             <van-cell-group>
                <van-field v-model="form.address" placeholder="详细地址" />
             </van-cell-group>
              <br>
             <van-button block @click="submitHandler" type="primary">保存</van-button>
         </div>
         </briup-fulllayout>
    </div>
</template>
<script>
import {mapState} from 'vuex'
import { post,response } from '../../../http/axios';
export default {
    computed:{
        ...mapState('user',['info'])
    },
    data(){
        return{
            form:{}
        }
    },
    methods:{
        submitHandler(){
            let url="/address/saveOrUpdate";
            //在提交前，将当前登录者的ID作为顾客ID
            this.form.customerId=this.info.id;
            post(url,this.form).then((response)=>{
                 //返回上一个页面
            this.$router.go(-1);
            this.$toast.success("添加成功");
            })
           
        }
    }    
}
</script>