<template>
  <div>
    <v-card elevation="2">
      <v-img
        class="white--text align-end"
        height="200px"
        :src="singleData.urlToImage"
      >
        <v-card-title>{{ singleData.title }}</v-card-title>
      </v-img>

      <v-card-subtitle class="pb-0">
        {{ singleData.description }}
      </v-card-subtitle>

      <v-card-text class="text--primary">{{ singleData.author }} </v-card-text>
      <h5 class="date-v">Published date : 20th January 2023</h5>
    </v-card>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'details',
  data() {
    return {
      data: [],
      singleData: [],
    }
  },
  async mounted() {
    this.fetchData(this.$route.params)
  },
  methods: {
    async fetchData(params) {
      await axios
        .get(
          'https://newsapi.org/v2/everything?q=bitcoin&apiKey=70905943afe0477ab21103fdbb396454'
        )
        .then((res) => (this.data = res.data.articles))

      this.singleData = this.data.find((e) => e.title === params.title)
    },
  },
}
</script>
<style lang="scss">
.date-v {
  margin-left: 16px;
  padding-bottom: 28px;
}
</style>
