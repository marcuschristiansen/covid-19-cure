<template>
<a-layout>
      <a-layout-header class="header">
        <img src="~/assets/logo.png" class="logo" alt="Is Covid 19 Cured">
      </a-layout-header>
      <a-layout-content>
        <h1 class=text-center>{{ date }}</h1>
        <a-row>
          <a-col :md="{ span: 12, offset: 6 }">
            <a-card v-for="(article, key) in newsArticles" :key="key" style="margin-bottom:2rem;">
              <img
                :alt="article.title"
                :src="article.urlToImage"
                slot="cover"
              />
              <a :href="article.url" target="_blank">
                <a-card-meta :title="article.title">
                  <template slot="description"
                    >{{ article.description }}</template
                  >
                </a-card-meta>
              </a>
            </a-card>
          </a-col>
        </a-row>
      </a-layout-content>
      <a-layout-footer class="footer">Copyright &copy; {{ year }}</a-layout-footer>
    </a-layout>
</template>

<script>
import axios from 'axios'
export default {
  mounted() {
    this.fetchNewsArticles()
  },
  data() {
    return {
      year: this.$moment().format('YYYY'),
      date: this.$moment().format('MMMM Do YYYY'),
      searchString: 'Covid-19 Cure',
      newsArticles: []
    }
  },
  props: {
    msg: String
  },
  methods: {
    async fetchNewsArticles() {
      await axios
        .get(`http://newsapi.org/v2/everything?qInTitle=${this.searchString}&sortBy=publishedAt&apiKey=6cc79d1255a44ad0b79a346ef065cf3d`)
        .then(response => (this.newsArticles = response.data.articles))
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.text-center {
  text-align: center;
}
.header {
  background-color: #182b3d;
  margin-bottom: 2rem;
}
.logo {
  display: inline-block;
  width: auto;
  height: 100%;
}
.footer {
  text-align: center;
}
</style>
