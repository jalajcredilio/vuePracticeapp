<template>
  <v-container fluid grid-list-md>
    <v-pagination
      v-model="page"
      :length="pages"
      @input="updatePage"
      circle
    ></v-pagination>
    <v-row no-gutters>
      <v-col
        v-for="(article, index) in historyList"
        :key="index"
        sm="2"
        md="6"
        lg="4"
      >
        <v-sheet class="pa-2 ma-2">
          <v-card width="100%" class="list-img">
            <v-img :src="article.urlToImage"></v-img>
            <v-card-title
              ><a :href="article.url">{{ article.title }}</a></v-card-title
            >
            <v-card-text>{{ article.author }}</v-card-text>
            <v-card-text
              ><h4>Published date :</h4>
              20th January 2023</v-card-text
            >
            <button @click="$router.push(`/details/${article.title}`)">
              view Details
            </button>
          </v-card>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      artical: [],
      page: 1,
      pageSize: 10,
      listCount: 0,
      historyList: [],
    }
  },
  async mounted() {
    this.fetchData()
  },
  computed: {
    pages() {
      if (this.pageSize == null || this.listCount == null) return 0
      return Math.ceil(this.listCount / this.pageSize)
    },
  },
  methods: {
    async fetchData() {
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
    initPage() {
      this.listCount = this.artical.length
      if (this.listCount < this.pageSize) {
        this.historyList = this.artical
      } else {
        this.historyList = this.artical.slice(0, this.pageSize)
      }
    },
    updatePage(pageIndex) {
      let start = (pageIndex - 1) * this.pageSize
      let end = pageIndex * this.pageSize
      this.historyList = this.artical.slice(start, end)
      this.page = pageIndex
      console.log(pageIndex, this.pageSize, 'pageIndex')
      console.log(this.historyList, 'historyList')
    },
  },
}
</script>
<style lang="scss">
.v-card__text {
  display: flex;
}
button {
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
