<template>
    <div>
        合同名称：<el-input v-model="form.filename" placeholder="请输入合同名称" style="width: 450px"></el-input>
        <el-button @click="toPdf()" type="info">点击生成合同</el-button>
    </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: "pdf",
        data(){
            return{
                form:{
                    filename:''
                },
                timer:''
            }
        },
        methods:{
            toPdf(){
                var filename=this.form.filename;
                window.open("http://localhost:9992/pdf/generate/"+filename,"_blank");
                clearTimeout(this.timer);  //清除延迟执行

                this.timer = setTimeout(()=>{   //设置延迟执行
                   axios.post("http://localhost:9992/pdf/hh/"+filename).then((res)=>{
                       if (res.data.success){
                           this.$message.success("pdf成功，请到本地磁盘里去查看！！！");
                       }else {
                           this.$message.error(res.data.msg);
                       }
                   })
                },9000);


            }
        },
        created() {

        }
    }
</script>

<style scoped>

</style>
