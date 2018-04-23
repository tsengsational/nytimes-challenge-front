<template lang="html">
  <div class="list" :class="{hide: toHide}">
    <div class="list-wrapper">
      <article-list-item v-for="(article, key) in articles" :article="article" :index="key" :language="language"/>
    </div>
  </div>
</template>

<script>
import ArticleListItem from "./ArticleListItem"

export default {
  components: {
    ArticleListItem
  },
  props: ["articles", "language", "currentView"],
  computed: {
    toHide: function() {
      console.log(this.currentView)
      return this.currentView === "List" ? false : true;
    }
  }
}
</script>

<style lang="scss">
  .list {
    width: 100%;
    &.hide {
      display: none;
    }
    .list-wrapper {
      position: relative;
      width: 100%;
    }
  }

  @media (min-width: 600px) {
    .list {
      .list-wrapper {
        width: 75%;
        left: 12.5%;
      }
    }
  }

  @media (min-width: 800px) {
    .list {
      .list-wrapper {
        width: 50%;
        left: 25%;

      }
    }
  }

</style>
