<template>
    <div>
        <el-table
                :data="tableData"
                border
                style="width: 100%">
            <el-table-column
                    prop="borrowId"
                    label="借款编号">
            </el-table-column>
            <el-table-column
                    prop="borrowName"
                    label="货款名称">
            </el-table-column>
            <el-table-column
                    prop="borrowRen"
                    label="借款人">
            </el-table-column>
            <el-table-column
                    prop="borrowMoney"
                    label="货款金额">
            </el-table-column>
            <el-table-column
                    prop="interestRate"
                    label="利率">
            </el-table-column>
            <el-table-column
                    prop="periods"
                    label="期数">
            </el-table-column>
            <el-table-column
                    prop="backWay"
                    label="还款方式">
            </el-table-column>
            <el-table-column
                    prop="borrowTime"
                    label="发布时间">
            </el-table-column>
            <el-table-column
                    prop="behoof"
                    label="借款用途">
            </el-table-column>
            <el-table-column
                    prop="borrowStatus"
                    label="申请状态">
                <template slot-scope="obj">
                    <span v-if="obj.row.borrowStatus==1">等待审批</span>
                    <span v-if="obj.row.borrowStatus==2">等待一审</span>
                    <span v-if="obj.row.borrowStatus==3">一审成功</span>
                    <span v-if="obj.row.borrowStatus==4">一审失败</span>
                    <span v-if="obj.row.borrowStatus==5">等待二审</span>
                    <span v-if="obj.row.borrowStatus==6">二审成功</span>
                    <span v-if="obj.row.borrowStatus==7">二审失败</span>
                </template>
            </el-table-column>
            <el-table-column
                    label="操作">
                <template slot-scope="obj">
                    <el-button v-if="obj.row.tid==0" type="success" @click="toJie(obj.row.borrowId)">接收</el-button>
                    <el-button v-if="obj.row.tid==1" type="info" @click="toShen(obj.row.borrowId)">审批</el-button>
                </template>
            </el-table-column>
        </el-table>
        <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :page-sizes="[6, 12, 18, 24]"
                :page-size="size"
                layout="total, sizes, prev, pager, next, jumper"
                :total="total"
                style="margin-left: 50px">
        </el-pagination>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        // name: "borrow.vue"
        data(){
            return{
                tableData:[],
                page:1,
                size:6,
                total:0,
                keyword:''
            }
        },
        methods:{
            query(){
                axios.get("http://localhost:9991/check/borrow/list?page="+this.page+"&size="+this.size+"&keyword="+this.keyword).then((res)=>{
                    if (res.data.success){
                        this.tableData=res.data.data.list;
                        this.total=res.data.data.total;
                    }else {
                        this.$message.error(res.data.message);
                    }
                })
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
                this.size=val;
                this.query();
            },
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
                this.page=val;
                this.query();
            },
            toJie(borrowId){
                alert(borrowId);
            },
            toShen(borrowId){
                alert(borrowId);
            }
        },
        created() {
            this.query();
        }
    }
</script>

<style scoped>
    .el-pagination {
        text-align: center;
    }
</style>
