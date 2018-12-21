<template>
    <div class="login">
        <el-form ref="form" :model="form" label-width="80px" :rules="rules" status-icon>
          <el-form-item label="用户名" prop="username">
            <el-input v-model="form.username" placeholder="请输入用户名"></el-input>
          </el-form-item>
            <el-form-item label="密码" prop="password">
            <el-input v-model="form.password" type="password"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="login">登录</el-button>
            <el-button class="reset" @click="reset">重置</el-button>
          </el-form-item>
        </el-form>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      form: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          { min: 3, max: 6, message: '长度在 3 到 6 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入用户密码', trigger: 'blur' },
          { min: 6, max: 12, message: '长度在 6 到 12 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
      this.$refs.form.validate(valid => {
        if (valid) {
          // 校验成功
          // 发送ajax请求进行登录
          // console.log('登录成功')
          axios({
            method: 'post',
            url: 'http://localhost:8888/api/private/v1/login',
            data: this.form
          }).then(res => {
            console.log(res.data)
            if (res.data.meta.status === 200) {
              this.$message.success('登录成功')
              localStorage.setItem('token', res.data.data.token)
              this.$router.push('/home')
            }
          })
        } else {
          console.log('登录失败')
        }
      })
    },
    reset () {
      // console.log(this.$refs.form)
      this.$refs.form.resetFields()
    }
  }
}
</script>

<style lang="less">
.login {
  height: 100%;
  width: 100%;
  background-color: #2d434c;
  overflow: hidden;

  .el-form {
    width: 400px;
    padding: 75px 40px 15px 40px;
    background-color: #fff;
    margin: 200px auto;
    border-radius: 20px;
    position: relative;
  }
  .reset {
    margin-left: 80px;
  }
}
</style>
