<template>
   <div>
       <el-form ref="form" :model="form" label-width="80px">
           <el-form-item label="产品名称">
               <el-input v-model="form.productName" style="width: 500px;"></el-input>
           </el-form-item>
           <el-form-item label="产品种类" >
               <el-select v-model="form.productType" placeholder="请选择">
                   <el-option
                           v-for="type in productType"
                           :key="type.typeName"
                           :label="type.typeName"
                           :value="type.typeName">
                   </el-option>
               </el-select>
           </el-form-item>
           <el-form-item label="贷款总额">
               <el-input v-model="form.capitalCount" style="width: 500px;"></el-input>
           </el-form-item>
           <el-form-item label="年利率">
               <el-input v-model="form.productRate" style="width: 500px;"></el-input>
           </el-form-item>
           <el-form-item label="期数">
               <el-input v-model="form.borrowCount" style="width: 500px;"></el-input>
           </el-form-item>
           <el-form-item label="还款方式">
               <el-select v-model="form.returnWay" placeholder="请选择">
                   <el-option
                           v-for="way in returnWay"
                           :key="way.returnWay"
                           :label="way.returnWay"
                           :value="way.returnWay">
                   </el-option>
               </el-select>
           </el-form-item>
           <el-form-item>
               <el-button type="primary" @click="edit">提交</el-button>
           </el-form-item>
       </el-form>
       {{form}}
   </div>
</template>

<script>
    //import axios from 'axios'
    export default {
        name: "add-product",
        data(){
            return{
                form:{

                },
                productType:[{typeName:"信用贷"},
                    {typeName:"抵押贷"}],
                returnWay:[{returnWay:'等额本息'},
                    {returnWay:'等额本金'}]
            }
        },
        methods:{
            edit(){
                axios.post("http://localhost:9105/v1/product/add",this.form).then((resp)=>{
                    alert("产品添加成功");
                })
            }
        },
        created() {

        }
    }
</script>

<style scoped>

</style>
