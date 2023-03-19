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
          神明赐予智者这新生之地。
        </v-card-subtitle>
        <v-card-text>
          <v-form>
            <v-text-field v-model="username" label="账户" hint="润云账户或氧化应用平台通用账户" placeholder="admin@runyun.cc" autofocus clearable></v-text-field>
            <v-text-field v-model="password" label="密码" hint="账户的密码" type="password" clearable></v-text-field>
            <v-checkbox label="会话持久化"></v-checkbox>
            <p>要使此功能生效，需授权子用户 OxygenGlobalSessionController 允许全局读写您的账户 Session。</p>
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
  name: 'IndexPage',
  head: {
    title: '登录',
  },
  data(){
    return {
      username:"",
      password:""
    }
  },
  methods: {
    showSnackBar(color,message){
      this.$refs.snackbar.snackBarPop(color,message)
    },
    loginTest () {
      if (this.username) {
        if (this.password) {
          this.$axios.post('/api/login', {
            username: this.username,
            password: this.password
          })
          .then(response => {
            if (response.data.code === 1200) {
              this.showSnackBar('success','登录成功！')
            } else if (response.data.code === 1201) {
              this.showSnackBar('error','密码错误。莓果键：' + response.data.berrykey)
            } else if (response.data.code === 1202) {
              this.showSnackBar('error','用户名不存在。莓果键：' + response.data.berrykey)
            }
          })
          .catch(error => {
            this.showSnackBar('error','接口错误。' + error)
          });
        } else {
          this.showSnackBar('warning','请输入密码。如果是第三方验证，请传入验证 key 。')
        }
      } else {
        this.showSnackBar('warning','请输入账户名。')
      }
      
    }
  },
}
</script>
