<template>
  <div class="login_container">
    <div class="login_box">
      <div class="avatar_box">
        <img src="../assets/logo.png"/>
      </div>
      <!--form-->
      <el-form ref="login_form_ref" class="login_form" :rules="login_form_rules" :model="login_form">
        <el-form-item prop="username">
          <el-input v-model="login_form.username" prefix-icon="el-icon-user" placeholder="账号名称"></el-input>
        </el-form-item>
        <el-form-item  prop="password">
          <el-input v-model="login_form.password" prefix-icon="el-icon-lock" type="password" placeholder="密码"></el-input>
        </el-form-item>
        <el-form-item class="login_btn">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      login_form: {
        username: 'admin',
        password: '123456'
      },
      login_form_rules: {
        username: [
          {
            required: true,
            message: '请输入账号名称',
            trigger: 'blur'
          },
          {
            min: 3,
            max: 35,
            message: '长度在 3 到 35 个字符',
            trigger: 'blur'
          }
        ],
        password: [
          {
            required: true,
            message: '请输入密码',
            trigger: 'blur'
          },
          {
            min: 6,
            max: 15,
            message: '长度在 6 到 15 个字符',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    // 重置表单
    resetLoginForm () {
      this.$refs.login_form_ref.resetFields()
    },
    login () {
      // 提交验证
      this.$refs.login_form_ref.validate(async valid => {
        if (!valid) return false
        const { data: result } = await this.$axios.post('/login', this.login_form)
        if (result.code === '0') {
          window.sessionStorage.setItem('token', result.data.token)
          this.$message.success('登录成功')
          this.$router.push('/home')
        } else {
          this.$message.info('登录失败')
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
  .login_container {
    background-color: #2b4b6b;
    height: 100%;
  }

  .login_box {
    width: 550px;
    height: 400px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    .avatar_box {
      height: 130px;
      width: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      box-shadow: 0 0 10px #ddd;
      padding: 10px;
      position: absolute;
      left: 50%;
      transform: translate(-50%,-50%);
      background-color: #fff;
      img{
        height: 100%;
        width: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
  }
  .login_form{
    width: 100%;
    position: absolute;
    bottom: 0;
    padding: 0 20px;
    box-sizing: border-box;
  }
  .login_btn{
    display: flex;
    justify-content: flex-end;
  }
</style>
