<template>
  <div class="container">
    <h2 class="title">用户注册</h2>
    <el-form class="form" :model="form" ref="form" :rules="rules" label-width="80px" :inline="false" size="normal">
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="form.email" placeholder="邮箱"></el-input>
      </el-form-item>
      <el-form-item label="验证码" prop="captcha">
        <el-row>
          <el-col :span="16">
            <el-input v-model="form.captcha" placeholder="验证码"></el-input>
          </el-col>
          <el-col :span="8">
            <img class="captcha" @click="refreshCaptcha" :src="captcha" alt="验证码" title="点击可刷新">
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">注册</el-button>
      </el-form-item>
    </el-form>
    
  </div>
</template>

<script>
export default {
  layout: 'blank',
  data() {
    return {
      captcha: '/api/user/captcha',
      form: {
        email: 'will',
        captcha: ''
      },
      rules: {
        email: [
          { required: true, message: '请输入邮箱' }
        ],
        captcha: [
          { required: true, message: '请输入验证码' }
        ]
      }
    }
  },
  methods: {
    refreshCaptcha() {
      this.captcha = `/api/user/captcha?_=${Date.now()}`
    },
    onSubmit() {
      this.$refs.form.validate(valid => {
        if (valid) {
          this.$http.post('/user/create', this.form).then(res => {
            if (res.code !== 0) {
              this.$message.error(res.message)
            } else {
              this.$message.success(res.message)
            }
          })
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  width: 500px;
  margin: 300px auto;
  .title {
    text-align: center;
  }
  .captcha {
    margin-left: 20px;
    height: 40px;
  }
}
</style>