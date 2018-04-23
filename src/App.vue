<template>
  <div id="app" :class="{list: list}">
    <div class="wrapper">
      <Instructions/>
      <Nav :handleToggle="handleToggle" :handleSelect="handleSelect" :handleButtonClick="handleButtonClick" :handleReset="handleReset" />
      <Carousel :articles="articles" :language="language" :resetPages="resetPages" :currentView="currentView" @pageFlip="handlePageFlip" :flipToIndex="flipToIndex"/>
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
import startArticles from './assets/startArticles'


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
      currentView: "Book",
      articleIndex: 0,
      flipToIndex: 0,
    }
  },
  computed: {
    list: function () {
      return this.currentView === "List" ? true : false;
    },
    wrapper: function () {
      return document.querySelector('.wrapper')
    }
  },
  methods: {
    handlePageFlip: function(index) {
      this.articleIndex = index + 1
    },
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
      // setting the currentView toggles between displaying the Carousel and List components
      let checked = e.target.checked
      if (checked) {
        // get all the article-list-item elements and find the element to scroll to.
        // get the offsetTop of the element and set the wrapper element's scroll position
        // to equal the desired element to be scrolled to.
        this.currentView = "List"
        let scroll = 0
        setTimeout( () => {
          let articleListItems = this.wrapper.querySelectorAll(".article-list-item")
          let articleToScroll = articleListItems[this.articleIndex]
          scroll = articleToScroll.offsetTop
          this.wrapper.scrollTop = scroll
        }, 5);
      } else {
        // get the scroll position of the wrapper element,
        // get all the article-list-item elements and determine which ones have been scrolled past.
        // Push all the elements scrolled past into an array.
        // Get the index of the last element in the array and set flipToIndex equal to that.
        // flipToIndex is a prop that is passed down to each Page component, which will
        // determine if each page should be flipped or not.
        let scrollY = this.wrapper.scrollTop
        let articleListItems = this.wrapper.querySelectorAll(".article-list-item")
        let articlesScrolledPast = []
        for (var i = 0; i < articleListItems.length; i++) {
          // test if current list item has been scrolled past,
          // if not, push current list item into articlesScrolledPast array
          if (articleListItems[i].offsetTop > scrollY) {
            break;
          }
          articlesScrolledPast.push(articleListItems[i])
        }
        if (articlesScrolledPast.length > 1) {
          this.flipToIndex = articlesScrolledPast.length - 1
        } else {
          this.flipToIndex = 0
        }
        this.wrapper.scrollTop = 0
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
