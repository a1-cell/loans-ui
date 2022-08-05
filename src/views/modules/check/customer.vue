<template>
    <div>
        <h1><a target="_blank" href="javascript:void(0)" @click="oo()">点击跳转客服</a></h1>
        <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
        <hr>
        <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
        <h1><a style="color: red" target="_blank" href="javascript:void(0)" @click="hh()">客服才有权限查看用户消息</a></h1>
    </div>
</template>

<script>
    export default {
        name: "customer",
        data(){
            return{

            }
        },
        methods:{
            oo(){
                window.open("http://localhost:9993/user1.html","_blank");
            },
          hh(){
              // location.href="http://localhost:9993/admin.html";
              window.open("http://localhost:9993/admin.html",'_blank');
          }
        },
        created() {

        }
    }
</script>

<style scoped>

</style>
