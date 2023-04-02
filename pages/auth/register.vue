<template>
<div>
  <SnackBar ref="snackbar" />
  <v-row>
    <v-col>
      <v-card>
        <v-card-title>
          注册
        </v-card-title>
        <v-card-subtitle>
          注册 Consolify 账户
        </v-card-subtitle>
        <v-card-text>
          <v-form>
            <v-text-field v-model="username" label="账户名" hint="您的 Consolify 账户名" placeholder="admin@runyun.cc" autofocus clearable></v-text-field>
            <v-text-field v-model="password" label="密码" hint="您的账户密码" type="password" clearable></v-text-field>
            <v-text-field v-model="email" label="电子邮箱" hint="可接收邮件的电子邮箱" placeholder="admin@runyun.cc" autofocus clearable></v-text-field>
            <v-btn @click="getVerifyCode">获取验证码</v-btn>
            <v-text-field v-model="verifyCode" label="验证码" hint="邮件验证码" type="number" clearable></v-text-field>
            <v-text-field v-model="sex" label="sex" type="number" clearable></v-text-field>
            <v-text-field v-model="respect" label="sex" type="number" clearable></v-text-field>
            <v-btn @click="regTest">注册</v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</div>
</template>

<script>
export default {
  name: 'IndexPage',
  data(){
    return {
      username: '',
      password: '',
      email: '',
      sex: 0,
      respect: 0,
      verifyCode: 0
    }
  },
  head: {
    title: '账户注册',
  },
  methods: {
    showSnackBar(color,message){
      this.$refs.snackbar.snackBarPop(color,message)
    },
    regTest () {
      if (this.username) {
        if (this.password) {
          this.$axios.post('/api/account/register', {
            username: this.username,
            password: this.password,
            email: this.email,
            sex: this.sex,
            respect: this.respect,
            verifyCode: this.verifyCode
          })
          .then(response => {
            if (response.data.code === 1000) {
              this.showSnackBar('success','注册成功！')
            } else {
              this.showSnackBar('error','注册失败！')
            }
          })
          .catch(error => {
            this.showSnackBar('error','注册失败。' + error.response.data.message + '。')
          });
        } else {
          this.showSnackBar('warning','请输入密码。')
        }
      } else {
        this.showSnackBar('warning','请输入账户名。')
      }  
    },
    getVerifyCode () {
      if (this.email) {
        this.$axios.post('/api/account/verifyCode', {
            username: this.username,
            password: this.password,
            email: this.email,
            activity: 'register',
            type: 'email'
          })
          .then(response => {
            if (response.data.code === 1000.1) {
              this.showSnackBar('success','发送验证码请求提交成功！请查收验证码。')
            } else {
              this.showSnackBar('warning','已请求发送验证码，但接口未给出正确响应。请查收验证码或重试。')
            }
          })
          .catch(error => {
            this.showSnackBar('error','发送验证码失败。' + error.response.data.message + '。请稍后重试。')
          });
      } else {
        this.showSnackBar('warning','请先填写电子邮箱。')
      }
    }
  },
}
</script>
