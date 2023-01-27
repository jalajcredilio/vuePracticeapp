<template>
  <v-container fluid grid-list-md>
    <v-pagination
      v-model="page"
      :length="pages"
      @input="updatePage"
      circle
    ></v-pagination>
    <v-row>
      <v-col
        v-for="(article, index) in historyList"
        :key="index"
        sm="2"
        md="6"
        lg="4"
      >
        <!-- <v-sheet class=""> -->
        <v-card height="100%">
          <v-img height="40%" :src="article.urlToImage"></v-img>
          <v-card-title
            ><a :href="article.url">{{ article.title }}</a></v-card-title
          >
          <v-card-text>{{ article.author }}</v-card-text>
          <v-card-text
            ><h4>Published date :</h4>
            20th January 2023</v-card-text
          >
          <v-btn @click="$router.push(`/details/${article.title}`)">
            view Details
          </v-btn>
        </v-card>
        <!-- </v-sheet> -->
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import axios from 'axios'
import Vue from 'vue'
import { SingleDataType } from '~/interface/productInterface'
export default Vue.extend(
  {
    data() {
      let artical: Array<SingleDataType> = []
      let page: number = 1
      let pageSize: number = 10
      let listCount: number = 0
      let historyList: Array<SingleDataType> = []
      let fetchData
      return {
        artical,
        page,
        pageSize,
        listCount,
        historyList,
      }
    },
    mounted() {
      this.fetchData()
    },
    computed: {
      pages(): number {
        if (this.pageSize == null || this.listCount == null) return 0
        return Math.ceil(this.listCount / this.pageSize)
      },
    },
    methods: {
      async fetchData(): Promise<void> {
        await axios
          .get(
            'https://newsapi.org/v2/everything?q=bitcoin&apiKey=70905943afe0477ab21103fdbb396454'
          )
          .then((res) => {
            this.artical = res.data.articles
          })
        this.initPage()
        this.updatePage(this.page)
      },
      initPage(): void {
        this.listCount = this.artical.length
        if (this.listCount < this.pageSize) {
          this.historyList = this.artical
        } else {
          this.historyList = this.artical.slice(0, this.pageSize)
        }
      },
      updatePage(pageIndex: number): void {
        let start = (pageIndex - 1) * this.pageSize
        let end = pageIndex * this.pageSize
        this.historyList = this.artical.slice(start, end)
        this.page = pageIndex
      },
    },
  }
) 
</script>
<style lang="scss">
.v-card__text {
  display: flex;
}
button {
  margin-bottom: 2%;
  margin-left: 18px;
  &.hover {
    color: #d8640ccc;
  }
}
.v-application a {
  color: #ffff;
  text-decoration: auto;

  &:hover {
    color: #d8640ccc;
  }
}
</style>
