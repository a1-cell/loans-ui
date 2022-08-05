<template>
  <div>
    <h3>当前投标状态:{{title}}</h3>
    <el-form ref="form" :model="form" label-width="80px">
      <el-form-item label="名称">
        <el-input v-model="form.name" style="width: 300px;"></el-input>
      </el-form-item>
      <el-form-item label="每次投标金额">
        <el-input v-model="form.money" style="width: 120px;"></el-input>元
        <el-button @click="add()" type="success" icon="el-icon-circle-plus-outline" circle></el-button>
        <el-button @click="minus()" type="success" icon="el-icon-remove-outline" circle v-if="form.money>50"></el-button>
        <span>（该数值须不小于50元，且为50的倍数）</span>
      </el-form-item>
      <el-form-item label="账户保留余额">
        <el-input v-model="form.retainmoney" style="width: 300px;"></el-input>元
      </el-form-item>
      <el-form-item label="利息范围">
        <el-input v-model="form.ratemin" style="width: 150px;"></el-input>%-
        <el-input v-model="form.ratemax" style="width: 150px;"></el-input>%
        <span>（10%-24%为有效利率范围）</span>
      </el-form-item>
      <el-form-item label="借款期限">
        <el-select v-model="form.periodsmin" placeholder="最低借款期限">
          <el-option label="3" value="3"></el-option>
          <el-option label="6" value="6"></el-option>
          <el-option label="9" value="9"></el-option>
          <el-option label="12" value="12"></el-option>
          <el-option label="18" value="18"></el-option>
          <el-option label="24" value="24"></el-option>
        </el-select>月-
        <el-select v-model="form.periodsmax" placeholder="最高借款期限">
          <el-option label="3" value="3"></el-option>
          <el-option label="6" value="6"></el-option>
          <el-option label="9" value="9"></el-option>
          <el-option label="12" value="12"></el-option>
          <el-option label="18" value="18"></el-option>
          <el-option label="24" value="24"></el-option>
        </el-select>月
      </el-form-item>
      <el-form-item label="最大借款金额">
        <el-input v-model="form.moneymax" style="width: 300px;"></el-input>元
      </el-form-item>
    </el-form>
    <el-button @click="ruleadd()" v-if="a" type="success" plain>开启自动投标</el-button>
    <el-button type="success" @click="norul()" plain>关闭自动投标</el-button>
    <el-divider></el-divider>
    <span v-if="b">请输入名称:</span><el-input v-model="name"  v-if="b" style="width: 300px;"></el-input>
    <el-button type="success" v-if="b" @click="getrule()" plain>查看标</el-button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "rule",
  data(){
    return{
      form:{
        money:50,
        companyid:1001
      },
      name:null,
      a:true,
      b:true,
      title:'查询后展示~~~'
    }
  },
  methods:{
    add:function (){
      this.form.money=parseInt(this.form.money)+parseInt(50);
    },
    minus:function (){
      this.form.money=parseInt(this.form.money)-parseInt(50);
    },
    //开始自动投标
    ruleadd:function (){
      if(this.a) {
        axios.post("http://localhost:9112/v1/addrule", this.form).then(resp => {
          if (resp.data.success) {
            this.a = false;
            this.b = true;
            alert("设置规则成功!");
          }
        })
      }else{
        alert("请刷新页面后在试~~~");
      }
    },
    //获取标
    getrule:function (){
      axios.get("http://localhost:9112/v1/getrule?name="+this.name).then(resp=>{
        console.log(resp)
        this.form=resp.data.data;
        if(resp.data.data.statue==1){
          this.title="正在自动投标"
        }else{
          this.title="自动投标结束"
        }
      })
    },
    //关闭自动投标
    norul:function (){
      axios.get("http://localhost:9112/v1/norul?name="+this.form.name).then(resp=>{
        console.log(resp);
        this.title="自动投标结束",
            this.form={
              money:50,
              companyid:1001
            }
      })
    }
  },
  created() {

  }
}
</script>

<style scoped>

</style>
