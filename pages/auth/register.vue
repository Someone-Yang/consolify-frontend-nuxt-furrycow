<template>
  <v-row>
    <v-col>
      <v-card>
        <v-card-title>
          注册
        </v-card-title>
        <v-card-subtitle>
          神明赐予智者这新生之地。
        </v-card-subtitle>
        <v-card-text>
          <v-form>
            <v-text-field label="账户" hint="润云账户或氧化应用平台通用账户" placeholder="admin@runyun.cc" autofocus clearable></v-text-field>
            <v-btn>注册</v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
}
</script>
