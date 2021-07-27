<template>
  <div class="container">
    <h2 class="title">用户登录</h2>
    <el-form class="form" :model="form" ref="form" :rules="rules" label-width="80px" :inline="false" size="normal">
      <el-form-item label="用户名" prop="nickname">
        <el-input v-model="form.nickname" placeholder="用户名"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="form.password" type="password" placeholder="密码"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">登录</el-button>
      </el-form-item>
    </el-form>
    
  </div>
</template>

<script>
import md5 from 'md5'
export default {
  layout: 'blank',
  data() {
    return {
      form: {
        nickname: 'will',
        password: 'wanchong',
      },
      rules: {
        nickname: [
          { required: true, message: 'nickname' }
        ],
        password: [
          { required: true, message: '请输入密码' }
        ]
      }
    }
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate(valid => {
        if (valid) {
          const params = {
            nickname: this.form.nickname,
            password: md5(this.form.password)
          }
          this.$http.post('/user/login', params).then(res => {
            if (res.code !== 0) {
              this.$message.error(res.message)
            } else {
              this.$message.success('登录成功')
              const { token, userId } = res.data
              localStorage.setItem('token', token)
              this.$http.get('/user/info', { params: { userId } }).then(res => {
                console.log(res.data)
              }).catch(err => {
                this.$message.err(err.message)
              })
              this.$router.push('/')
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
}
</style>