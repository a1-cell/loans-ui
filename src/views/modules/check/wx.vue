<template>
    <div>
        access_token:
                <textarea v-model="form.token" style="width: 450px;height: 70px"></textarea>
                <el-button type="success" @click="toGetToken()">获取token</el-button>

    </div>
</template>

<script>
    import axios from 'axios';
    export default {
        name: "wx",
        data(){
            return{
                form:{
                    token:''
                }
            }
        },
        methods:{
            toGetToken(){
                // axios.get("http://localhost:9991/notemq/wxs").then((res)=>{
                //     this.form.token=res.data;
                // })
            }
        },
        created() {

        }
    }
</script>

<style scoped>

</style>
