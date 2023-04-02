<template>
  <v-row>
    <v-col>
      跳转中
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  head: {
    title: '首页',
  },
  mounted() {
    this.$router.push({
      path: '/table'
   })
  }
}
</script>
