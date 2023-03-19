<template>
  <v-row>
    <v-col>
      载入中
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
      path: `/auth/login`
   })
  }
}
</script>
