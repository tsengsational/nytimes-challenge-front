<template lang="html">
  <div class="carousel">
    <div class="first" >
      <articleCard class="top" :article="this.topArticle" :language="language"/>
    </div>
    <div class="pages" >
      <page v-for="(page, key) in pageArticles" v-if="key % 2 === 0" :key="key" :index="key" :resetPages="resetPages" :articles="pageArticles" :language="language" />
    </div>
    <div class="hidden">
      <articleCard v-for="(article, key) in articles" :key="key" :article="article" :language="language" />
    </div>
  </div>
</template>

<script>
import ArticleCard from "./Article"
import Page from "./Page"

export default {
  props: ["articles", 'resetPages', 'language'],
  components: {
    ArticleCard,
    Page
  },
  computed: {
    topArticle: function() {
      return this.articles[0]
    },
    pageArticles: function() {
      let pageArticles = this.articles.slice(1, this.articles.length)
      console.log("pageArticles: ", pageArticles)
      return pageArticles
    }
  }
}
</script>

<style lang="scss">
  @import "../assets/settings.scss";

  .hidden {
    opacity: 0;
  }
  .carousel {
    box-sizing: border-box;
    position: relative;
    height: $article_height;
  }
  .first {
    position: absolute;
    top: 0;
    left: 0;
  }

  @media (min-width: 450px) {
    .first {
      top: 0;
      left: 0;
    }
  }
</style>
