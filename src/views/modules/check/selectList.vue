<template>
    <div>
        <el-table
                :data="tableData"
                border
                style="width: 100%">
            <el-table-column
                    prop="credutorId"
                    label="转让编号">
            </el-table-column>
            <el-table-column
                    prop="title"
                    label="标题">
            </el-table-column>
            <el-table-column
                    prop="tranRen"
                    label="转让人">
            </el-table-column>
            <el-table-column
                    prop="money"
                    label="本金">
            </el-table-column>
            <el-table-column
                    prop="rateXi"
                    label="利息">
            </el-table-column>
            <el-table-column
                    prop="tranMoney"
                    label="转让金">
            </el-table-column>
            <el-table-column
                    prop="rate"
                    label="利率">
            </el-table-column>
            <el-table-column
                    prop="daiHuan"
                    label="代还/总期数">
            </el-table-column>
            <el-table-column
                    prop="credutorStatus"
                    label="状态">
            </el-table-column>
            <el-table-column
                    prop="borrowRen"
                    label="借款人">
            </el-table-column>
            <el-table-column
                    label="操作">
                <template slot-scope="obj">
                    <el-button type="success" @click="toSet(obj.row.credutorId)">接手</el-button>
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

        <el-dialog
                title="支付"
                :visible.sync="dialogVisible"
                width="60%">
            <span>请您支付<font color="red">{{form.money}}</font>元转让金</span>
            <span><br><br><br><br><br><br></span>
            <el-button type="success" @click="toWx()">微信支付</el-button>
            <el-button type="success" @click="toZfb()">支付宝支付</el-button>
            <el-button type="success" @click="toQt()">其他支付</el-button>
            <span slot="footer" class="dialog-footer">
  </span>
        </el-dialog>

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
                dialogVisible:false,
                form:{
                    money:0.0,
                    credutorId:''
                }
            }
        },
        methods:{
            query(){
                axios.get("http://localhost:9992/check/credutorList?page="+this.page+"&size="+this.size).then((res)=>{
                    if (res.data.success){
                        // console.log(res);
                        this.tableData=res.data.data.list;
                        this.total=res.data.data.total;
                    }else {
                        this.$message.error(res.data.msg);
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
            toSet(credutorId){
                this.form={
                    money:0.0,
                    credutorId:''
                };
                axios.get("http://localhost:9992/check/getPrice/"+credutorId).then((res)=>{
                    if (res.data.success){
                        // console.log(res);
                        this.form.money=res.data.data.tranMoney;
                        this.form.credutorId=credutorId;

                    }else {
                        this.$message.error(res.data.msg);
                    }
                })
                this.dialogVisible=true;
            },
            toWx(){
                alert("假的，先没做");
                var qq=this.form.money;
                    this.moni(qq);
                this.dialogVisible=false;
            },
            toZfb(){
                alert("假的，先没做");
                var qq=this.form.money;
                this.moni(qq);
                this.dialogVisible=false;
            },
            toQt(){
                // this.$router.push("/hh");
                location.href="http://localhost:8001/#/check-hh";
                this.dialogVisible=false;
            },
            moni(qq){
                var uid=4;
                axios.post("http://localhost:9992/check/addSet/"+this.form.credutorId+"/"+uid+"/"+qq).then((res)=>{
                    if (res.data.success){
                        this.$message.success(res.data.msg);
                        this.query();

                    }else {
                        this.$message.error(res.data.msg);
                    }
                })
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
