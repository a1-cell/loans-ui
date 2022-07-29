<template>
    <div>
        <form method="post" action="http://localhost:9113/bha-neo-app/lanmaotech/service">
            <input name="serviceName"  v-model="reqData.serviceName">
            <input name="platformNo"  v-model="reqData.platformNo">
            <input name="reqData"  v-model="reqData.reqData">
            <input name="keySerial"  v-model="reqData.keySerial">
            <input name="sign"  v-model="reqData.sign"><br>
            <el-button type="primary" @click="toAibank">投标</el-button>
        </form>
    </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: "test",
        data(){
            return{
                reqData:{
                    serviceName:'',
                    platformNo:'',
                    reqData:{
                        requestNo:'',
                        platformUserNo:'',
                        bizType:'',
                        Amount:'',
                        expired:''
                    },
                    keySerial:null,
                    sign:''
                }
            }
        },
        methods:{
            //开户
            toAibank(){
                document.forms[0].submit();
            }
        },
        created() {
            axios.post("http://localhost:9113/bha-neo-app/lanmaotech/initParam2").then((resp)=>{
                console.log(resp)
                this.reqData = resp.data;
            });
        }
    }
</script>

<style scoped>

</style>
