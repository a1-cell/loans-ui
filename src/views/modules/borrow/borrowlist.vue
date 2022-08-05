<template>
  <div>
    <el-table
        :data="tableData"
        border
        style="width: 100%">
      <el-table-column
          prop="borrowName"
          label="借款标题">
      </el-table-column>
      <el-table-column
          prop="borrowRen"
          label="借款人">
      </el-table-column>
      <el-table-column
          prop="borrowMoney"
          label="借款金额">
      </el-table-column>
      <el-table-column
          prop="behoof"
          label="借款用途">
      </el-table-column>
      <el-table-column
          prop="housename"
          label="抵押物信息">
      </el-table-column>
      <el-table-column
          fixed="right"
          label="操作">
        <template slot-scope="scope">
          <el-button type="success" v-if="scope.row.housename!=null" @click="getmoney(scope.row.housename)">爬取价格</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog
        title="社区价格请参考!"
        :visible.sync="dialogVisible"
        width="60%">
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="社区名称">
          <el-input v-model="name"></el-input>
        </el-form-item>
        <el-form-item label="社区价格">
          <el-input v-model="form"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
  </span>
    </el-dialog>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "borrowlist",
  data(){
    return{
      tableData:[],
      dialogVisible:false,
      name:'',
      form:{}
    }
  },
  methods:{

    //获取借款申请列表
    borrowlist:function (){
      axios.get("http://localhost:9112/v1/getlist").then(resp=>{
        console.log(resp);
        this.tableData=resp.data.data;
      })
    },
    //爬取价格
    getmoney:function (name){
      this.name=name;
      axios.get("http://localhost:9113/bha-neo-app/lanmaotech/spider/"+name).then(resp=>{
        console.log(resp);
        this.dialogVisible=true;
        this.form=resp.data.data;
      })
    }
  },
  created() {
    this.borrowlist()
  }
}
</script>

<style scoped>

</style>
