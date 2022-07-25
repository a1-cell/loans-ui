<template>
  <div>
    <el-table
        :data="tableData"
        border
        style="width: 100%">
      <el-table-column
          prop="userid"
          label="用户编号">
      </el-table-column>
      <el-table-column
          prop="name"
          label="昵称">
      </el-table-column>
      <el-table-column
          prop="usernum"
          label="信用积分">
      </el-table-column>
      <el-table-column
          prop="money"
          label="可用余额">
      </el-table-column>
      <el-table-column
          prop="creattime"
          label="注册时间">
      </el-table-column>
      <el-table-column
          fixed="right"
          label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small">查看</el-button>
          <el-button type="text" size="small">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "user",
  data(){
    return{
      tableData:[]
    }
  },
  methods:{
    //查询用户列表
    list:function (){
      axios.get("http://localhost:9111/v1/user").then(resp=>{
        this.tableData=resp.data.data;
      })
    }
  },
  created() {
    this.list();
  }
}
</script>

<style scoped>

</style>
