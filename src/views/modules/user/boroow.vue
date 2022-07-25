<template>
  <div>
    <el-form ref="form" :model="form" label-width="80px">
      <el-form-item label="借款标题">
        <el-input v-model="form.borrowName"></el-input>
      </el-form-item>
      <el-form-item label="借款金额">
        <el-input v-model="form.borrowMoney"></el-input>
      </el-form-item>
      <el-form-item label="年利率">
        <el-input v-model="form.interestRate"></el-input>
      </el-form-item>
      <el-form-item label="借款期限">
        <el-select v-model="form.periods" placeholder="借款期限">
          <el-option label="3个月" value="3个月"></el-option>
          <el-option label="6个月" value="6个月"></el-option>
          <el-option label="9个月" value="9个月"></el-option>
          <el-option label="12个月" value="12个月"></el-option>
          <el-option label="18个月" value="18个月"></el-option>
        <el-option label="24个月" value="24个月"></el-option>
      </el-select>
      </el-form-item>
      <el-form-item label="还款方式">
        <el-select v-model="form.backWay" placeholder="还款方式">
          <el-option label="等额本息" value="等额本息"></el-option>
          <el-option label="等额本金" value="等额本金"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="借款用途">
       <el-select v-model="form.behoof" placeholder="借款用途">
        <el-option label="短期周转" value="短期周转"></el-option>
        <el-option label="购房借款" value="购房借款"></el-option>
        <el-option label="个人消费" value="个人消费"></el-option>
        <el-option label="装修借款" value="装修借款"></el-option>
        <el-option label="投资创业" value="投资创业"></el-option>
        <el-option label="其他借款" value="其他借款"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="借款产品">
        <el-select v-model="form.pid" placeholder="请选择">
          <el-option
              v-for="item in options"
              :key="item.id"
              :label="item.productType"
              :value="item.id">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="身份证正面照片">
        <el-upload
            class="avatar-uploader"
            action="http://localhost:9112/v1/upload"
            :show-file-list="false"
            :on-success="handleAvatarSuccess"
            :before-upload="beforeAvatarUpload">
          <img v-if="form.cardUrl1!='' " :src="form.cardUrl1" class="avatar">
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>

      </el-form-item>
      <el-form-item label="身份证反面照片">
        <el-upload
            class="avatar-uploader"
            action="http://localhost:9112/v1/upload"
            :show-file-list="false"
            :on-success="handleAvatarSuccess2"
            :before-upload="beforeAvatarUpload">
          <img v-if="form.cardUrl2!='' " :src="form.cardUrl2" class="avatar">
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
      </el-form-item>
    </el-form>
    <el-button type="success" plain @click="add()">贷款</el-button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "boroow",
  data(){
    return{
      form:{
        cardUrl1:'',
        cardUrl2:''
      },
      options:[]
    }
  },
  methods:{
    //贷款
    add:function (){
      axios.post("http://localhost:9112/v1/add",this.form).then(resp=>{
        if(resp.data.success){
          alert(resp.data.msg);
          this.form={
            cardUrl1:'',
            cardUrl2:''
          }
        }else{
          alert("借款失败!");
        }
      })
    },
    //查询产品
    optionslist:function (){
      axios.get("http://localhost:9105/v1/list").then(resp=>{
        console.log(resp)
        this.options=resp.data.data;
      })
    },
    handleAvatarSuccess(res, file) {
      this.form.cardUrl1=res;
    },
    handleAvatarSuccess2(res, file) {
      this.form.cardUrl2=res;
    },
    beforeAvatarUpload(file) {
      const isJPG =true;
      const isLt2M = file.size / 1024 / 1024 < 10;

      if (!isJPG) {
        this.$message.error('上传头像图片只能是 JPG 格式!');
      }
      if (!isLt2M) {
        this.$message.error('上传头像图片大小不能超过 2MB!');
      }
      return isJPG && isLt2M;
    }
  },
  created() {
    this.optionslist();
  }
}
</script>
<style>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409EFF;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>

<style scoped>

</style>
