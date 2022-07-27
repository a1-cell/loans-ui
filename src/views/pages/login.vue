<template>
  <div class="aui-wrapper aui-page__login">
    <div class="aui-content__wrapper">
      <main class="aui-content">
        <div class="login-header">
          <h2 class="login-brand">{{ $t('brand.lg') }}</h2>
        </div>
        <div class="login-body">
          <h3 class="login-title">{{ $t('login.title') }}</h3>
          <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmitHandle()" status-icon>
            <el-form-item prop="username">
              <el-input v-model="dataForm.username" :placeholder="$t('login.username')">
                <span slot="prefix" class="el-input__icon">
                  <svg class="icon-svg" aria-hidden="true"><use xlink:href="#icon-user"></use></svg>
                </span>
              </el-input>
            </el-form-item>
            <el-form-item prop="password">
              <el-input v-model="dataForm.password" type="password" :placeholder="$t('login.password')">
                <span slot="prefix" class="el-input__icon">
                  <svg class="icon-svg" aria-hidden="true"><use xlink:href="#icon-lock"></use></svg>
                </span>
              </el-input>
            </el-form-item>
            <el-form-item prop="captcha">
              <el-row :gutter="20">
                <el-col :span="14">
                  <el-input v-model="dataForm.captcha" :placeholder="$t('login.captcha')">
                    <span slot="prefix" class="el-input__icon">
                      <svg class="icon-svg" aria-hidden="true"><use xlink:href="#icon-safetycertificate"></use></svg>
                    </span>
                  </el-input>
                </el-col>
                <el-col :span="10" class="login-captcha">
                  <img :src="captchaPath" @click="getCaptcha()">
                </el-col>
              </el-row>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="dataFormSubmitHandle()" class="w-percent-100">{{ $t('login.title') }}</el-button>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" class="w-percent-100" @click="sign()">去注册</el-button>
            </el-form-item>
<!--            第三方登录：<a href="http://openapi.baidu.com/oauth/2.0/authorize?client_id=kUZ1XIRxWtP9wLBNG6pS7fZp&response_type=code&redirect_uri=http://localhost:8080/baidu">百度</a>-->
            <div>第三方登陆：<a @click="otherlogin" href="http://openapi.baidu.com/oauth/2.0/authorize?response_type=code&client_id=0KarGjdTCIDKlbifVwmSLG1s&redirect_uri=http://localhost/baidu"><img src="baidu.png" style="width: 35px; height: 35px"></a></div>
          </el-form>
        </div>
        <div class="login-footer">
          <p>
            <a href="http://demo.open.renren.io/renren-security" target="_blank">{{ $t('login.demo') }}</a>
          </p>
          <p><a href="https://www.renren.io/" target="_blank">{{ $t('login.copyright') }}</a>2022 © renren.io</p>
        </div>
      </main>
    </div>
    <el-dialog
        title="请输入注册基本信息"
        :visible.sync="dialogVisible"
        width="60%">
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="账号">
          <el-input v-model="form.username"></el-input>
        </el-form-item>
        <el-form-item label="密码">
          <el-input v-model="form.userpassword"></el-input>
        </el-form-item>
        <el-form-item label="手机号">
          <el-input v-model="form.userphone"></el-input>
        </el-form-item>
        <el-form-item label="昵称">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="adduser()">确 定</el-button>
  </span>
    </el-dialog>
  </div>
</template>

<script>
import Cookies from 'js-cookie'
import debounce from 'lodash/debounce'
import { getUUID } from '@/utils'
import axios from "axios";
export default {
  data () {
    return {
      captchaPath: '',
      dataForm: {
        username: 'admin',
        password: 'admin',
        uuid: '',
        captcha: ''
      },
      dialogVisible:false,
      form:{}

    }
  },
  computed: {
    dataRule () {
      return {
        username: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        password: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        captcha: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ]
      }
    }
  },
  created () {
    this.getCaptcha()
  },
  methods: {
    // 获取验证码
    getCaptcha () {
      this.dataForm.uuid = getUUID()
      this.captchaPath = `${window.SITE_CONFIG['apiURL']}/captcha?uuid=${this.dataForm.uuid}`
    },
    //注册
    sign:function (){
      this.dialogVisible=true;
    },
    //添加
    adduser:function (){
     // alert(0)
      axios.post("http://localhost:9111/v1/adduser",this.form).then(resp=>{
          if(resp.data.success){
            alert(resp.data.msg);
            this.dialogVisible=false;
          }else{
            alert(resp.data.msg);
          }
      })
    },
    //第三方登录
    otherlogin:function(){
      this.$router.replace({ name: 'home' })
    },
    // 表单提交
    dataFormSubmitHandle: debounce(function () {
      this.$refs['dataForm'].validate((valid) => {
        if (!valid) {
          return false
        }
        this.$http.post('/login', this.dataForm).then(({ data: res }) => {
          if (res.code !== 0) {
            this.getCaptcha()
            return this.$message.error(res.msg)
          }
          Cookies.set('token', res.data.token)
          this.$router.replace({ name: 'home' })
        }).catch(() => {})
      })
    }, 1000, { 'leading': true, 'trailing': false })
  }
}
</script>
