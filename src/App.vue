<template>
  <div id="app">
    <Instructions/>
    <Nav :handleSelect="handleSelect" :handleButtonClick="handleButtonClick" :handleReset="handleReset" />
    <Carousel :articles="articles" :language="language" :resetPages="resetPages" />
    <Footer/>
  </div>
</template>

<script>
import Nav from './components/Nav'
import Carousel from './components/Carousel'
import config from './config'
import Footer from './components/Footer'
import Instructions from './components/Instructions'

var startArticles = [
  {
    english: {
      headline: "",
      url: "",
      byline: "",
      summary: "",
      images: [],
      type: ""
    },
    martian: {
      headline: "",
      url: "",
      byline: "",
      summary: "",
      images: [],
      type: ""
    }
  },
  {
    english: {
      headline: "",
      url: "",
      byline: "",
      summary: "",
      images: [],
      type: ""
    },
    martian: {
      headline: "",
      url: "",
      byline: "",
      summary: "",
      images: [],
      type: ""
    }
  },
  {
    english: {
      headline: "",
      url: "",
      byline: "",
      summary: "",
      images: [],
      type: ""
    },
    martian: {
      headline: "",
      url: "",
      byline: "",
      summary: "",
      images: [],
      type: ""
    }
  },
  {
    english: {
      headline: "",
      url: "",
      byline: "",
      summary: "",
      images: [],
      type: ""
    },
    martian: {
      headline: "",
      url: "",
      byline: "",
      summary: "",
      images: [],
      type: ""
    }
  },
  {
    english: {
      headline: "",
      url: "",
      byline: "",
      summary: "",
      images: [],
      type: ""
    },
    martian: {
      headline: "",
      url: "",
      byline: "",
      summary: "",
      images: [],
      type: ""
    }
  }
]

export default {
  name: 'app',
  components: {
    Nav,
    Carousel,
    Footer,
    Instructions
  },
  methods: {
    handleButtonClick: function () {
      if (this.fetchedArchives === false) {
        this.fetchedArchives = true;
        let url = this.getURL("/more")
        this.getArticles(url, "update")
      }
    },
    handleReset: function() {
      this.resetPages = !this.resetPages
      setTimeout(function () {
        this.resetPages = !this.resetPages
      }, 600);
    },
    handleSelect: function (e) {
      let value = e.target.value
      this.language = value
    },
    getURL: function(route = "/api") {
      let url = config.endpoint
      return url += route
    },
    getArticles: function(url, action = null) {
      return fetch(url)
        .then(response => {
          return response.json()
        })
        .then(json => {
          switch (action) {
            case "start":
              this.resetPages = true
              this.populateStart(json);
              setTimeout(() => {
                this.resetPages = false
              }, 600);
              break;
            case "update":
              this.populateArticles(json);
              break;
            default:
              console.log('no action')
              break;
          }
        })
    },
    populateArticles: function(json) {
      const articles = json
      this.articles = this.articles.concat(articles)
    },
    populateStart: function(json) {
      const articles = json
      this.articles = articles
    }
  },
  data: function(){
    return {
      fetchedArchives: false,
      articles: startArticles,
      resetPages: false,
      language: "English"
    }
  },
  created: function(){
    let url = this.getURL()
    this.getArticles(url, "start")
  }
}
</script>

<style lang="scss">
@import "assets/settings.scss";

#app {
  height: 100vh;
  overflow: hidden;
  font-family: $font_header;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body {
  margin: 0;
}
</style>
