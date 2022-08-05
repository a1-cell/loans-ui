<template>
  <div>
    <el-form ref="form" :model="coupon" label-width="80px">
      <el-form-item label="发布券名">
        <el-input v-model="coupon.cname"></el-input>
      </el-form-item>
      <el-form-item label="券的类型">
        <el-radio-group v-model="coupon.tid">
          <el-radio label="1">加息</el-radio>
          <el-radio label="2">减息</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="利率">
        <el-input v-model="coupon.interest"></el-input>
      </el-form-item>
      <el-form-item label="使用类型">
        <el-select v-model="coupon.type" placeholder="请选择使用类型">
          <el-option label="电脑官网" value="1"></el-option>
          <el-option label="手机公众号" value="2"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="领取方式">
        <el-radio-group v-model="coupon.grante">
          <el-radio label="1">官网</el-radio>
          <el-radio label="2">公众号</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="发行量">
        <el-input v-model="coupon.num"></el-input>
      </el-form-item>
      <el-form-item label="有效天数">
        <el-input v-model="coupon.days"></el-input>
      </el-form-item>
      <el-form-item label="活动时间">
        <el-col :span="5">
          <el-date-picker type="datetime" placeholder="选择开始日期" v-model="coupon.createTime" style="width: 100%;"></el-date-picker>
        </el-col>
        <el-col class="line" :span="2">-----------------</el-col>
        <el-col :span="5">
          <el-date-picker type="datetime" placeholder="选择结束时间" v-model="coupon.endTime" style="width: 100%;"></el-date-picker>
        </el-col>
      </el-form-item>

      <el-form-item label="最少投资">
        <el-input v-model="coupon.money"></el-input>
      </el-form-item>
      <el-form-item label="最大投资">
        <el-input v-model="coupon.maxMoney"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">立即创建</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>

  </div>

</template>

<script>
import mixinViewModule from '@/mixins/view-module'
import AddOrUpdate from './role-add-or-update'
import axios from "axios";
export default {
  name: "coupon",
  mixins: [mixinViewModule],
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: '/sys/role/page',
        getDataListIsPage: true,
        deleteURL: '/sys/role',
        deleteIsBatch: true
      },
      dataForm: {
        name: ''
      },
      coupon:{}
    }
  },
  components: {
    AddOrUpdate
  },
  methods:{
    test(){
      axios.get('http://localhost:9011/test/test1').then(resp=>{
        console.log(resp)
      })
    },
    onSubmit(){
      axios.post('http://localhost:9111/coupon/add',this.coupon).then(resp=>{
        console.log(resp)
        if(resp.data){
          this.$message.success("发布成功，请等待审核")
          location.reload();
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