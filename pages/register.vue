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
            <img class="captcha" :src="captcha" alt="验证码">
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
  async created() {
    const res = await this.$axios.$get('/api/user/info')
    console.log(res)
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate(valid => {
        if (valid) {
          alert('ok')
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