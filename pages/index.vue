<template>
<a-layout>
      <a-layout-header class="header">
        <img src="~/assets/logo.png" class="logo" alt="Is Covid 19 Cured">
      </a-layout-header>
      <a-layout-content>
        <h1 class="text-center">{{ date }}</h1>
        <a-row class="text-center" style="margin-bottom:2rem;">
          <a-col :md="{ span: 12, offset: 6 }">
            <span v-if="statistics">
              <a-tag>Cases: {{ formatNumber(statistics.data.confirmed) }}</a-tag>
              <a-tag color="#2db7f5">Active: {{ formatNumber(statistics.data.active) }}</a-tag>
              <a-tag color="#f50">Deaths: {{ formatNumber(statistics.data.deaths) }}</a-tag>
              <a-tag color="#87d068">Recovered: {{ formatNumber(statistics.data.recovered) }}</a-tag>
            </span>
          </a-col>
        </a-row>
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
    this.fetchStatusUpdate()
    this.fetchStatistics()
  },
  data() {
    return {
      year: this.$moment().format('YYYY'),
      date: this.$moment().format('MMMM Do YYYY'),
      statistics: null,
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
        .get(`https://newsapi.org/v2/everything?qInTitle=${this.searchString}&sortBy=publishedAt&apiKey=6cc79d1255a44ad0b79a346ef065cf3d`)
        .then(response => (this.newsArticles = response.data.articles))
    },
    async fetchStatusUpdate() {
      this.statusCureNotFound()
    },
    async fetchStatistics() {
      await axios
        .get('https://covid2019-api.herokuapp.com/v2/total')
        .then(response => (this.statistics = response.data))
    },
    statusCureFound() {
      return this.$notification.success({
          message: 'Status Update - CURE FOUND',
          description:
            'A cure has been found for Covid-19.',
          duration: 0
      });
    },
    statusCureNotFound() {
      return this.$notification.error({
          message: 'Status Update - NO CURE FOUND',
          description:
            'At this stage no cure treatment is available to treat Covid-19.',
          duration: 0
      });
    },
    formatNumber(num) {
      return num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,')
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
