<template>
    <div>
        <div style="margin-left: 900px;margin-top: 230px">
           <h1>恭喜您来到博天金融YYDS还款页面</h1>
            <h3 style="margin-left: 80px">本次页面绝对让你体会到地狱的风采</h3>
            <h1 style="margin-left: 130px">痛苦吧！！！</h1>
            <a href="javascript:void(0)" v-if="this.ss==''" @click="xuan()" style="color: red;margin-left: 180px">选择</a>
            <span v-if="this.ss!=''" style="color: royalblue;margin-left: 150px;font-size: 30px">选择成功</span>
        </div>
        <hr>
        <div style="margin-left: 960px;margin-top: 20px">
            <el-button type="success" plain @click="changeType(1)">微信支付</el-button>
            <el-button type="primary" plain @click="changeType(2)">支付宝支付</el-button>
        </div>

        <el-dialog
                title="提示"
                :visible.sync="dialogVisible"
                width="60%">

            <el-table
                    ref="multipleTable"
                    :data="tableData"
                    tooltip-effect="dark"
                    style="width: 100%"
                    @selection-change="handleSelectionChange">
                <el-table-column
                        type="selection"
                        width="55">
                </el-table-column>
                <el-table-column
                        prop="borrowId"
                        label="编号ID">
                </el-table-column>
                <el-table-column
                        prop="borrowName"
                        label="借贷名称">
                </el-table-column>
                <el-table-column
                        prop="borrowMoney"
                        label="本金">
                </el-table-column>
            </el-table>

            <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="Que()">确 定</el-button>
  </span>
        </el-dialog>
    </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: "pay",
        data(){
            return{
                form:{
                    type:'',
                    id:''
                },
                dialogVisible:false,
                tableData:[],
                multipleSelection: [],
                ss:''
            }
        },
        methods:{
            changeType(type){
                //根据订单编号调用支付接口
                var id=this.form.id;
                if (type==1){
                    window.open("http://localhost:9995/pay.html?id="+id,"_blank");
                    // location.href="http://localhost:9995/pay.html?id="+id;
                }else {
                    window.open("http://localhost:9995/pay/alipay?id="+id,"_blank");
                    // location.href="http://localhost:9995/pay/alipay?id="+id;
                }
                this.ss=''
            },
            xuan(){
                axios.get("http://localhost:9995/pay/findAll").then((res)=>{
                    if (res.data.success){
                        // console.log(res);
                        this.tableData=res.data.data;

                    }else {
                        this.$message.error(res.data.msg);
                    }
                })
                this.dialogVisible=true;
            },
            Que(){
                for (var i in this.multipleSelection) {
                    var obj=this.multipleSelection[i];
                    this.form.id=obj.borrowId;
                }
                console.log(this.form.id);
                this.ss="1";
                this.dialogVisible=false;
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            }
        },
        created() {
            this.ss='';
        }
    }
</script>

<style scoped>

</style>
