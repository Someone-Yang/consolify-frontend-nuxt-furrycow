<template>
<div>
  <SnackBar ref="snackbar" />
  <v-row>
    <v-col cols="12" md="8">
      <v-row>
        <v-col>
          <v-card>
            <v-card-title>账户概览</v-card-title>
            <v-card-text v-if="overviewLoaded">
              <p>欢迎您，{{ username }}</p>
              <p>联系邮箱：{{ useremail }}</p>
            </v-card-text>
            <v-card-text v-else>
              <v-progress-circular indeterminate></v-progress-circular>
              <p>个人信息正在加载中</p>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-col>
    <v-col cols="12" md="4">
      <v-row>
        <v-col>
          <v-card>
            <v-card-title>账户概览</v-card-title>
            <v-card-text v-if="overviewLoaded">
              <p>欢迎您，{{ username }}</p>
              <p>联系邮箱：{{ useremail }}</p>
            </v-card-text>
            <v-card-text v-else>
              <v-progress-circular indeterminate></v-progress-circular>
              <p>个人信息正在加载中</p>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-card>
            <v-card-title>账户资产</v-card-title>
            <v-card-text v-if="overviewLoaded">
              <p>{{ userprice }}</p>
            </v-card-text>
            <v-card-text v-else>
              <v-progress-circular indeterminate></v-progress-circular>
              <p>个人信息正在加载中</p>
            </v-card-text>
            <v-card-actions>
              <v-btn to="/bill" text>账单管理</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</div>
</template>

<script>
export default {
  name: 'TablePage',
  layout: 'table',
  data(){
    return {
      overviewLoaded: false,
      username: '',
      useremail: '',
      userprice: 0
    }
  },
  head: {
    title: '工作台',
  },
  mounted () {
    this.$axios.get('/api/account/overview')
    .then(response => {
      this.username = response.data.data.username
      this.useremail = response.data.data.email
      this.usereprice = response.data.data.price
      this.overviewLoaded = true
    })
    .catch(error => {
      this.showSnackBar('error','获取个人信息失大败！' + error.response.data.message + '。')
    });
  },
  methods: {
    showSnackBar(color,message){
      this.$refs.snackbar.snackBarPop(color,message)
    },
  },
}
</script>
