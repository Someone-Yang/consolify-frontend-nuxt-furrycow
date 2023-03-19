<template>
<v-snackbar
  v-model="snackbar"
  :color=snackbarColor
  right
  top
  @showSnackBar="snackBarPop"
>
  {{ snackbarText }}

  <template #action="{ attrs }">
    <v-btn
      icon
      v-bind="attrs"
      @click="snackbar = false"
    >
      <v-icon>
        mdi-close
      </v-icon>
    </v-btn>
  </template>
</v-snackbar>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      snackbar: false,
      snackbarColor: "info",
      snackbarText: "",
    }
  },
  methods: {
    snackBarPop (color, message) {
      if (this.snackbar === true) {
        this.snackbar = false
      }
      this.snackbarColor = color
      this.snackbarText = message
      this.snackbar = true
    }
  }
}
</script>