<template>
<v-dialog
  v-if="loading"
  v-model="dialog"
  max-width="240"
>
  <v-card>
    <v-container>
      <v-row no-gutters>
        <v-spacer></v-spacer>
        <v-col cols="auto">
            <v-progress-circular
              indeterminate
            ></v-progress-circular>
        </v-col>
        <v-spacer></v-spacer>
      </v-row>
      <v-row no-gutters>
        <v-col cols="12">
          <div class="text-center text-h6">ホームページ解析中</div>
          <div class="text-center text-caption">所要時間約20秒(最長1,2分)</div>
        </v-col>
      </v-row>
    </v-container>
  </v-card>
</v-dialog>
</template>

<script>
export default {
  data() {
    return {
      loading: false,
      dialog: true
    }
  },
  methods: {
    start() {
      this.loading = true
    },
    finish() {
      this.loading = false
    }
  }
}
</script>