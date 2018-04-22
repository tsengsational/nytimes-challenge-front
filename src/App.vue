<template>
  <div id="app" :class="{list: list}">
    <div class="wrapper">
      <Instructions/>
      <Nav :handleToggle="handleToggle" :handleSelect="handleSelect" :handleButtonClick="handleButtonClick" :handleReset="handleReset" />
      <Carousel :articles="articles" :language="language" :resetPages="resetPages" :currentView="currentView"/>
      <List :currentView="currentView" :articles="articles" :language="language"/>
      <Footer/>
    </div>
  </div>
</template>

<script>
import Nav from './components/Nav'
import Carousel from './components/Carousel'
import config from './config'
import Footer from './components/Footer'
import Instructions from './components/Instructions'
import List from './components/List'

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
    Instructions,
    List
  },
  data: function(){
    return {
      fetchedArchives: false,
      articles: startArticles,
      resetPages: false,
      language: "English",
      currentView: "Book"
    }
  },
  computed: {
    list: function () {
      return this.currentView === "List" ? true : false;
    }
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
    handleToggle: function (e) {
      let checked = e.target.checked
      if (checked) {
        this.currentView = "List"
      } else {
        let wrapper = document.querySelector('.wrapper')
        wrapper.scrollTop = 0
        this.handleReset()
        this.currentView = "Book"
      }
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
  created: function(){
    let url = this.getURL()
    this.getArticles(url, "start")
  }
}
</script>

<style lang="scss">
@import "assets/settings.scss";

#app {
  &.list {
    .wrapper {
      overflow: auto;
    }
  }
  .wrapper {
    box-sizing: border-box;
    overflow: hidden;
    height: 100vh;
    width: 100vw;
  }
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
