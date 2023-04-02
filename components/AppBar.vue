<template>
  <v-app-bar fixed app>
    <v-toolbar-title @click="$router.push({path: '/table'})">润云数据 Consolify</v-toolbar-title>
    <v-subheader>{{ title }}</v-subheader>
    <v-spacer />
  </v-app-bar>
</template>

<script>
export default {
  name: 'AppBar',
  props:{
    title:{
      type: String,
      default: '润云 Consolify'
    }
  }
}
</script>
