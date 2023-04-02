<template>
<div>
  <SnackBar ref="snackbar" />
  <v-row>
    <v-col>
      <v-card>
        <v-card-title>支付</v-card-title>
        <v-card-text>
          <v-text-field v-model="payId" label="订单号" type="text" clearable></v-text-field>
          <v-btn @click="goPay">支付</v-btn>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
  <v-dialog v-model="showDialog">
    <v-card>
      <v-card-title>订单信息获取成功</v-card-title>
      <v-card-text>
        <p>您的订单可以支付。</p>
        <p>订单号：{{ payId }}</p>
        <p>订单地址：{{ payUrl }}</p>
        <p>请在弹出的窗口扫码完成支付，或点击下方按钮跳转到支付页面。</p>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn text :href="payUrl" target="_blank">前往支付</v-btn>
        <v-btn text color="primary" @click="showDialog = false">我已支付完成</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</div>
</template>

<script>
export default {
  name: 'PayPage',
  layout: 'bill',
  data(){
    return {
      payId: '',
      preSet: false,
      showDialog: false,
      payUrl: ''
    }
  },
  head: {
    title: '支付',
  },
  mounted(){
    if (this.$route.params.payId) {
      this.payId = this.$route.params.payId
      this.preSet = true
      this.showDialog = true
      this.goPay ()
    }
  },
  methods: {
    showSnackBar(color,message){
      this.$refs.snackbar.snackBarPop(color,message)
    },
    goPay () {
      this.$axios.post('/api/pay', {
        uuid: this.payId,
      })
      .then(response => {
        this.payUrl = response.data.data.url
        this.showDialog = true
        this.showSnackBar('success','获取订单信息成功！')
        window.open(this.payUrl,"_blank")
      })
      .catch(error => {
        this.showSnackBar('error','获取订单信息失败。' + error.response.data.message + '。请稍后再试。')
      });
    }
  },
}
</script>
