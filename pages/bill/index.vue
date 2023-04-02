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
        <v-card-title>账单
          <v-spacer></v-spacer>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="搜索账单"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-data-table
          :headers="billTableHeader"
          :items="bills"
          items-per-page=10
          class="elevation-1"
          :search="search"
          :loading="!billLoaded"
          loading-text="正在获取账单信息，请稍后"
        >
          <template #[`item.status`]="{ item }">
            <v-chip v-if="item.status === 0" color="orange">未支付</v-chip>
            <v-chip v-else-if="item.status === 1" color="green">已支付</v-chip>
            <v-chip v-else-if="item.status === -1" color="grey">已取消</v-chip>
            <v-chip v-else color="grey">未知</v-chip>
          </template>
          <template #[`item.payType`]="{ item }">
            <v-chip v-if="item.payType === 'AliPay'">支付宝</v-chip>
            <v-chip v-else-if="item.payType === 'WechatPay'">微信钱包</v-chip>
            <v-chip v-else>未知</v-chip>
          </template>
          <template #[`item.actions`]="{ item }">
            <v-btn text color="primary" @click="goPay(item.uuid)">详情</v-btn>
            <v-btn v-if="item.status === 0" text @click="goPay(item.uuid)">支付</v-btn>
          </template>
        </v-data-table>
      </v-card>
    </v-col>
  </v-row>
</div>
</template>

<script>
export default {
  name: 'BillPage',
  layout: 'bill',
  data(){
    return {
      useruuid: '',
      userprice: 0,
      billLoaded: false,
      overviewLoaded: false,
      search:'',
      bills: [],
      billTableHeader: [
        {text: '订单号', value: 'uuid'},
        {text: '产品名称', value: 'product.name'},
        {text: '支付方式', value: 'payType'},
        {text: '状态', value: 'status'},
        {text: '订单金额', value: 'product.price'},
        {text: '操作', value: 'actions'},
      ]
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
      this.$axios.post('/api/pay/getUserInvoice', {
        type: 'user',
        uuid: this.useruuid,
      })
      .then(response => {
        this.bills = response.data.data
        this.billLoaded = true
      })
      .catch(error => {
        this.showSnackBar('error','获取订单信息失败。' + error.response.data.message + '。请稍后再试。')
      });
    })
    .catch(error => {
      this.showSnackBar('error','获取个人信息失大败！' + error.response.data.message + '。')
    });
  },
  methods: {
    showSnackBar(color,message){
      this.$refs.snackbar.snackBarPop(color,message)
    },
    goPay(uuid){
      this.$router.push({
        path: '/bill/pay/' + uuid
      })
    }
  },
}
</script>
