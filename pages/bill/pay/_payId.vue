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
</div>
</template>

<script>
export default {
  name: 'IndexPage',
  data(){
    return {
      payId: '',
      preSet: false
    }
  },
  head: {
    title: '支付',
  },
  mounted(){
    if (this.$route.params.payId) {
      this.payId = this.$route.params.payId
      this.preSet = true
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
        this.showSnackBar('success','发起订单成功！订单：' + response.data.data.url + '。')
      })
      .catch(error => {
        this.showSnackBar('error','获取订单信息失败。' + error.response.data.message + '。请稍后再试。')
      });
    }
  },
}
</script>
