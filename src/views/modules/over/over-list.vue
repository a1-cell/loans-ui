<template>
    <div>
        <el-table :data="table" border style="width: 100%">
            <el-table-column prop="oid" label="编号"></el-table-column>
            <el-table-column prop="userid" label="逾期人员编号"></el-table-column>
            <el-table-column prop="tel" label="电话号码"></el-table-column>
            <el-table-column prop="overday" label="逾期天数"></el-table-column>
            <el-table-column label="操作">
                <template slot-scope="obj">
                    <el-button type="success" @click="cui(obj.row)">手动催收</el-button>
                </template>
            </el-table-column>
        </el-table>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: "over-list",
        data(){
            return{
                table:[],
            }
        },
        methods:{
            getList(){
                axios.get("http://localhost:9205/v1/list").then((resp)=>{
                    if(resp.data.success){
                        this.table=resp.data.data;
                    }
                })
            },
            cui(obj){
                axios.post("http://localhost:9205/v1/cui",obj).then((resp)=>{
                    if(resp.data.success){
                        this.$message.success(resp.data.msg);
                        this.getList();
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
