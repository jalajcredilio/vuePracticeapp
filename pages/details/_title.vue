<template>
  <div>
    <v-card elevation="2">
      <v-img
        class="white--text align-end"
        :src="product.urlToImage"
      >
        <v-card-title>{{ product.title }}</v-card-title>
      </v-img>

      <v-card-subtitle class="pb-0">
        {{ product.description }}
      </v-card-subtitle>

      <v-card-text class="text--primary">{{ product.author }} </v-card-text>
      <h5 class="date-v">Published date : 20th January 2023</h5>
    </v-card>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import { Dictionary } from 'vue-router/types/router'
import {SingleDataType} from '../../interface/productInterface'
export default {
  name: 'details',
  data() {
    const data: Array<SingleDataType> = []
    let product: SingleDataType = {
      description: '',
      urlToImage: '',
      title: '',
      author: ''
    }

    return {
      data,
      product,
    }
  },
  async mounted() {
    this.fetchData(this.$route.params)
  },
  methods: {
    async fetchData(params: Dictionary<String>): Promise<void> {
      await axios
        .get(
          'https://newsapi.org/v2/everything?q=bitcoin&apiKey=70905943afe0477ab21103fdbb396454'
        )
        .then((res) => (this.data = res.data.articles))
        const filterData: SingleDataType | any = this.data.find((e: SingleDataType) => e.title === params.title)
        if(filterData) {
          this.product = filterData 
        }
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
