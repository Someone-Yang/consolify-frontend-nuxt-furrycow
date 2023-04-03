<template>
<div>
  <SnackBar ref="snackbar" />
  <v-row>
    <v-col>
      <v-card>
        <v-card-title>
          登录
        </v-card-title>
        <v-card-subtitle>
          登录到你干嘛哎哟的 Consolify
        </v-card-subtitle>
        <v-card-text>
          <v-form>
            <v-text-field v-model="username" label="账户" hint="您的 Consolify 账户名" placeholder="admin@runyun.cc" autofocus clearable></v-text-field>
            <v-text-field v-model="password" label="密码" hint="您的账户密码" type="password" clearable></v-text-field>
            <v-btn @click="loginTest">登录</v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</div>
</template>

<script>
export default {
  name: 'LoginPage',
  layout: 'auth',
  data(){
    return {
      username: '',
      password: ''
    }
  },
  head: {
    title: '账户登录',
  },
  methods: {
    showSnackBar(color,message){
      this.$refs.snackbar.snackBarPop(color,message)
    },
    loginTest () {
      if (this.username) {
        if (this.password) {
          this.$axios.post('/api/account/login', {
            username: this.username,
            password: this.password
          })
          .then(response => {
            if (response.data.code === 1000) {
              this.showSnackBar('success','登录成功！正在跳转到工作台。')
              setTimeout(() => this.$router.push({path: '/table'}),1000)
            } else {
              this.showSnackBar('error','登录失败！')
            }
          })
          .catch(error => {
            this.showSnackBar('error','登录失败。' + error.response.data.message + '。')
          });
        } else {
          this.showSnackBar('warning','请输入密码。')
        }
      } else {
        this.showSnackBar('warning','请输入账户名。')
      }
      
    }
  },
}
</script>
