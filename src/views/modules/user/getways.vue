<template>
  <div>
    <form method="post" action="http://localhost:9113/bha-neo-app/lanmaotech/gateway">
      <input name="serviceName"  v-model="reqDatas.serviceName">
      <input name="platformNo"  v-model="reqDatas.platformNo">
      <input name="reqData"  v-model="reqDatas.reqData">
      <input name="keySerial"  v-model="reqDatas.keySerial">
      <input name="sign"  v-model="reqDatas.sign"><br>
      <el-button type="primary" @click="toAibank">立即开户</el-button>
    </form>
    {{reqDatas.reqData}}
  </div>
</template>

<script>
export default {
  name: "getways",
  data(){
    return{
      reqDatas:{
        serviceName:'PERSONAL_REGISTER_EXPAND',
        platformNo:'2236568989',
        reqData:{
         platformUserNo:'123456789',
          requestNo:'123456',
          userRole:'BORROWERS',
          checkType:' LIMIT',
          redirectUrl:'http://aibank.5gzvip.91tunnel.com/bha-neo-app/lanmaotech'
        },
        keySerial:1,
        sign:'test'
      }
    }
  },
  methods:{
    //开户
    toAibank(){
      alert(0)
      document.forms[0].submit();
    }
  },
  created() {

  }
}
</script>

<style scoped>

</style>
