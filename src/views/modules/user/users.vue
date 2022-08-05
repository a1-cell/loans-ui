<template>
  <div>
    <el-table
        :data="tableData"
        border
        style="width: 100%">
      <el-table-column
          prop="username"
          label="用户名称">
      </el-table-column>
      <el-table-column
          prop="userphone"
          label="手机号">
      </el-table-column>
      <el-table-column
          prop="creattime"
          label="注册时间">
      </el-table-column>
      <el-table-column
          fixed="right"
          label="是否实名认证">
        <template slot-scope="scope">
          <span v-if="scope.row.realstatue==1">已认证</span>
          <span v-if="scope.row.realstatue==0">未认证</span>
        </template>
      </el-table-column>
      <el-table-column
          fixed="right"
          label="是否升级为企业">
        <template slot-scope="scope">
          <span v-if="scope.row.companystatue==1">已升级</span>
          <span v-if="scope.row.companystatue==0">未升级</span>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "users",
  data(){
    return{
      tableData:[]
    }
  },
  methods:{
    users:function (){
      axios.get("http://localhost:9111/v1/getuser").then(resp=>{
        console.log(resp)
        this.tableData=resp.data.data;
      })
    }
  },
  created() {
    this.users()
  }
}
</script>

<style scoped>

</style>
