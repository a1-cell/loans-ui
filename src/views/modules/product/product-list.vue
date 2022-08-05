<template>
    <div>
        <el-table :data="table" border style="width: 100%">
            <el-table-column prop="productName" label="产品名称"></el-table-column>
            <el-table-column prop="productType" label="产品类型"></el-table-column>
            <el-table-column prop="productRate" label="利率"></el-table-column>
            <el-table-column prop="borrowCount" label="期数"></el-table-column>
            <el-table-column prop="returnWay" label="还款方式"></el-table-column>
            <el-table-column prop="createTime" label="产品上线时间"></el-table-column>
            <el-table-column prop="capitalCount" label="资金总额"></el-table-column>
        </el-table>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: "product-list",
        data(){
            return{
                table:[],
            }
        },
        methods:{
            getList(){
                axios.get("http://localhost:9105/v1/list").then((resp)=>{
                    if(resp.data.success){
                        this.table=resp.data.data;
                    }
                })
            }
        },
        created() {
            this.getList();
        }
    }
</script>

<style scoped>

</style>
