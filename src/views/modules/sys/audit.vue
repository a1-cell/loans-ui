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
        </span>
      </el-table-column>
      <el-table-column
          fixed="right"
          label="操作"
          width="100">
        <template slot-scope="scope">
          <el-button @click="handleClick(scope.row)" type="text" size="small">审核</el-button>
        </template>
      </el-table-column>
    </el-table>


  </div>
</template>

<script>
import mixinViewModule from '@/mixins/view-module'
import AddOrUpdate from './role-add-or-update'
import axios from "axios";
export default {
  name: "audit",
  mixins: [mixinViewModule],
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: '/sys/menu/list',
        deleteURL: '/sys/menu'
      },
      coupon:[]
    }
  },
  components: {
    AddOrUpdate
  },
  methods:{
    getList(){
      axios.get('http://localhost:9111/coupon/getList').then(resp=>{
        console.log(resp)
        this.coupon=resp.data.data
      })
    },
    handleClick(row){
      const h = this.$createElement;
      this.$msgbox({
        title: '审核',
        message: h('p', null, [
          h('span', null, '是否通过 '),
        ]),
        showCancelButton: true,
        confirmButtonText: '通过',
        cancelButtonText: '拒绝',
        beforeClose: (action, instance, done) => {
          if (action === 'confirm') {
            console.log("通过")

            axios.get("http://localhost:9111/coupon/upStutue?cid="+row.cid).then(resp=>{
              console.log(resp)
            })
            done();
          } else {

            axios.get("http://localhost:9111/coupon/upStatue?cid="+row.cid).then(resp=>{
              console.log(resp)
            })

            done();
          }
        }
      }).then(action => {
        this.$message({
          type: 'info',
          message: 'action: ' + action
        });
      });
    }
  },
  created() {
    this.getList()
  }
}
</script>

<style scoped>

</style>