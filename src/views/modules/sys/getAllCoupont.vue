<template>
  <div>
    <el-table
        :data="coupon"
        border
        style="width: 100%">
      <el-table-column
          prop="cid"
          label="发起人"
          width="150">
      </el-table-column>
      <el-table-column
          prop="cname"
          label="券名称"
          width="120">
      </el-table-column>
      <el-table-column
          prop="interest"
          label="利率"
          width="120">
      </el-table-column>
      <el-table-column
          prop="tid"
          label="券类型"
          width="120">
        <span slot-scope="scope">
          <span v-if="scope.row.tid===1">加息</span>
          <span v-if="scope.row.tid===2">减息</span>
        </span>
      </el-table-column>
      <el-table-column
          prop="type"
          label="使用范围"
          width="120">
        <span slot-scope="scope">
          <span v-if="scope.row.type===1">官网</span>
          <span v-if="scope.row.type===2">公众号</span>
        </span>
      </el-table-column>

      <el-table-column
          prop="grante"
          label="发行方式"
          width="120">
        <span slot-scope="scope">
          <span v-if="scope.row.grante===1">官网</span>
          <span v-if="scope.row.grante===2">公众号</span>
        </span>
      </el-table-column>
      <el-table-column
          prop="num"
          label="发行数量"
          width="120">
      </el-table-column>
      <el-table-column
          prop="days"
          label="领取后的有效天数"
          width="120">
      </el-table-column>
      <el-table-column
          prop="money"
          label="最小投资"
          width="120">
      </el-table-column>
      <el-table-column
          prop="maxMoney"
          label="最大投资"
          width="120">
      </el-table-column>
      <el-table-column
          prop="createTime"
          label="抢券开始时间"
          width="120">
      </el-table-column>
      <el-table-column
          prop="endTime"
          label="抢券结束时间"
          width="120">
      </el-table-column>
      <el-table-column
          prop="statue"
          label="审核状态"
          width="120">
        <span slot-scope="scope">
          <span v-if="scope.row.statue==='0'">未审核</span>
          <span v-if="scope.row.statue==='1'">审核通过</span>
          <span v-if="scope.row.statue==='2'">审核拒绝</span>
          <span v-if="scope.row.statue==='3'">过期</span>
        </span>
      </el-table-column>
    </el-table>

  </div>
</template>

<script>
import mixinViewModule from '@/mixins/view-module'
import AddOrUpdate from './menu-add-or-update'
import axios from "axios";
export default {
  name: "getAllCoupont",
  mixins: [mixinViewModule],
  data () {
    return {
      coupon:[]
    }
  },
  components: {
    AddOrUpdate
  },
  created() {
    this.getList();
  },
  methods:{
    getList(){
      axios.get('http://localhost:9111/coupon/getNewList').then(resp=>{
        console.log(resp)
        this.coupon=resp.data.data
      })
    }
  }
}
</script>

<style scoped>

</style>