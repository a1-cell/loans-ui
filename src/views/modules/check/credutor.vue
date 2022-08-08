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
                    label="借贷人">
            </el-table-column>
            <el-table-column
                    prop="borrowMoney"
                    label="货款本金">
            </el-table-column>
            <el-table-column
                    prop="interestRate"
                    label="货款利率">
            </el-table-column>
            <el-table-column
                    prop="borrowTime"
                    label="发布时间">
            </el-table-column>
            <el-table-column
                    prop="creStatus"
                    label="是否转债">
                <template slot-scope="obj">
                    <span v-if="obj.row.creStatus==0">未转债...</span>
                    <span v-if="obj.row.creStatus==1">转债中...</span>
                </template>
            </el-table-column>
            <el-table-column
                    prop="uname"
                    label="资方代表">
            </el-table-column>
            <el-table-column
                    label="操作">
                <template slot-scope="obj">
                    <el-button type="success" @click="toCredutor(obj.row.borrowId)">发起转债</el-button>
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
                title="借贷信息详情"
                :visible.sync="dialogVisible"
                width="60%">


            <el-form ref="form" :model="form" label-width="80px">
                <el-form-item>
                    <h1>该信息状态：{{form.credutorStatus}}...</h1>
                </el-form-item>
                <el-form-item hidden>
                    <el-input hidden v-model="form.borrowId" style="width: 450px"></el-input>
                </el-form-item>
                <el-form-item label="借款标题">
                    <el-input v-model="form.title" style="width: 450px" readonly></el-input>
                </el-form-item>
                <el-form-item label="转让人">
                    <el-input v-model="form.tranRen" style="width: 450px" readonly></el-input>
                </el-form-item>
                <el-form-item label="本金">
                    <el-input v-model="form.money" style="width: 450px" readonly></el-input>
                </el-form-item>
                <el-form-item label="利率">
                    <el-input v-model="form.rate" style="width: 450px" readonly></el-input>
                </el-form-item>
                <el-form-item label="利息">
                    <el-input v-model="form.rateXi" style="width: 450px" readonly></el-input>
                </el-form-item>
                <el-form-item label="待还/总期数">
                    <el-input v-model="form.daiHuan" style="width: 450px" readonly></el-input>
                </el-form-item>
                <el-form-item label="借款人">
                    <el-input v-model="form.borrowRen" style="width: 450px" readonly></el-input>
                </el-form-item>
                <el-form-item label="转让金">
                    <el-input v-model="form.tranMoney" placeholder="请输入转让金" style="width: 450px"></el-input>
                </el-form-item>
            </el-form>

            {{form}}
            <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="toQue()">确 定</el-button>
  </span>
        </el-dialog>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        data(){
            return{
                tableData:[],
                page:1,
                size:6,
                total:0,
                keyword:'',
                dialogVisible:false,
                form:{
                    borrowId:'',
                    title:'',
                    tranRen:'',
                    money:0.0,
                    rateXi:0.0,
                    tranMoney:null,
                    rate:0.0,
                    daiHuan:'',
                    credutorStatus:'',
                    borrowRen:''
                }
            }
        },
        methods:{
            query(){
                // localStorage.getItem("userid");
                var userid=3;
                axios.post("http://localhost:9992/check/credutor?page="+this.page+"&size="+this.size+"&userid="+userid).then((res)=>{
                    if (res.data.success){
                        console.log(res);
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
            toCredutor(borrowId){
                this.form={
                    borrowId:'',
                    title:'',
                    tranRen:'',
                    money:0.0,
                    rateXi:0.0,
                    tranMoney:null,
                    rate:0.0,
                    daiHuan:'',
                    credutorStatus:'',
                    borrowRen:''
                };
                axios.post("http://localhost:9992/check/creBack/"+borrowId).then((res)=>{
                    if (res.data.success){
                        console.log(res);
                        this.form.borrowId=res.data.data.borrowId;
                        this.form.borrowRen=res.data.data.borrowRen;
                        this.form.title=res.data.data.borrowName;
                        this.form.tranRen=res.data.data.uname;
                        this.form.money=res.data.data.borrowMoney;
                        this.form.rateXi=8000;
                        this.form.rate=res.data.data.interestRate;
                        this.form.daiHuan=7;
                        this.form.credutorStatus=res.data.data.cstatus;
                    }else {
                        this.$message.error(res.data.msg);
                    }
                })
                this.dialogVisible=true;


            },
            toQue(){
                axios.post("http://localhost:9992/check/addCredutor",this.form).then((res)=>{
                    if (res.data.success){
                        this.$message.success(res.data.msg);
                    }else {
                        this.$message.error(res.data.msg);
                    }
                })
                this.query();
                this.dialogVisible = false
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
