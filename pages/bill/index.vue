<template>
<div>
  <SnackBar ref="snackbar" />
  <v-row>
    <v-col>
      <v-card>
        <v-card-title>余额</v-card-title>
        <v-card-text v-if="overviewLoaded">
          {{ userprice }}
        </v-card-text>
        <v-card-text v-else>
          <v-progress-circular indeterminate></v-progress-circular>
          <p>正在获取余额</p>
        </v-card-text>
        <v-card-actions>
          <v-btn to="/bill/addfund">充值</v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
  <v-row>
    <v-col>
      <v-card>
        <v-card-title>账单</v-card-title>
        <v-card-text v-if="billLoaded">
          
        </v-card-text>
        <v-card-text v-else>
          <v-progress-circular indeterminate></v-progress-circular>
          <p>正在获取订单信息，请稍后</p>
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
      useruuid: '',
      userprice: 0,
      billLoaded: false,
      overviewLoaded: false
    }
  },
  head: {
    title: '账单中心',
  },
  mounted(){
    this.$axios.get('/api/account/overview')
    .then(response => {
      this.useruuid = response.data.data.uuid
      this.userprice = response.data.data.price
      this.overviewLoaded = true
    })
    .catch(error => {
      this.showSnackBar('error','获取个人信息失大败！' + error.response.data.message + '。')
    });
    this.$axios.post('/api/pay/getUserInvoice', {
        type: 'user',
        uuid: this.useruuid,
      })
      .then(response => {
        console.log(response.data)
        this.billLoaded = true
      })
      .catch(error => {
        this.showSnackBar('error','获取订单信息失败。' + error.response.data.message + '。请稍后再试。')
      });
  },
  methods: {
    showSnackBar(color,message){
      this.$refs.snackbar.snackBarPop(color,message)
    },
  },
}
</script>
