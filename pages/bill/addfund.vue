<template>
<div>
  <SnackBar ref="snackbar" />
  <v-row>
    <v-col>
      <v-card>
        <v-card-title>创建充值订单</v-card-title>
        <v-card-text>
          <v-text-field v-model="price" label="金额" type="number" clearable></v-text-field>
          <v-select v-model="payType" label="支付方式" :items=paySelectItems item-value="type" item-text="text"></v-select>
          <v-btn @click="createBill">充值</v-btn>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
  <v-dialog v-model="showDialog">
    <v-card>
      <v-card-title>您的订单已准备好</v-card-title>
      <v-card-text>
        <p>您的订单已准备好！</p>
        <p>订单号：{{ payId }}</p>
        <p>点击下方按钮前往支付。</p>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn text :to="toPayPath">前往支付</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</div>
</template>

<script>
export default {
  name: 'IndexPage',
  data(){
    return {
      price: 0,
      payType: '',
      paySelectItems: [
        {type: 'WechatPay',text: '微信'},
        {type: 'AliPay',text: '支付宝'}
      ],
      showDialog: false,
      payId: '',
      toPayPath: ''
    }
  },
  head: {
    title: '充值',
  },
  methods: {
    showSnackBar(color,message){
      this.$refs.snackbar.snackBarPop(color,message)
    },
    createBill () {
      this.$axios.post('/api/pay/createInvoice', {
        payType: this.payType,
        uuid: null,
        instanceConfig: {
          type: 'addfunds',
          price: this.price
        }
      })
      .then(response => {
        this.showSnackBar('success','发起订单成功！订单：' + response.data.data.uuid + '。')
        this.payId = response.data.data.uuid
        this.showDialog = true
        this.toPayPath = '/bill/pay/' + this.payId
      })
      .catch(error => {
        this.showSnackBar('error','发起订单失败。' + error.response.data.message + '。')
      });
    }
  },
}
</script>
