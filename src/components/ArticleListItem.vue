<template lang="html">
  <div class="article-list-item">
    <div class="headline">
      <div class="image" v-if="hasMultimedia" v-bind:style="imageStyle" >
      </div>
      <span v-html="this.activeArticle.headline" ></span>
    </div>
    <div class="byline">
      {{activeArticle.byline}}
    </div>
    <div class="snippet">
      <span class="text" v-html="this.activeArticle.summary" ></span><span v-if="this.activeArticle.summary.length > 0"><a :href="this.activeArticle.url" >Read more...</a></span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["article", "language"],
  computed: {
    activeArticle: function() {
      switch (this.language) {
        case "English":
          return this.article.english
          break;
        case "Martian":
          return this.article.martian
          break;
        default:
          break;
      }
    },
    hasMultimedia: function() {
      if (this.activeArticle.images) {
        return this.activeArticle.images.length > 0 ? true : false;
      } else {
        return false
      }
    },
    largeImage: function() {
      return this.activeArticle.images[0].types.find(function(element){
        return element.type === "jumbo"
      });
    },
    thumbImage: function() {
      return this.activeArticle.images[0].types.find(function(element){
        return element.type === "thumbStandard"
      });
    },
    imageStyle: function() {
      if (window.innerWidth >= 450) {
        return {
          backgroundImage: `url(https://static01.nyt.com/${this.largeImage.content})`,
          backgroundPosition: 'center',
          backgroundRepeat: 'no-repeat',
          width: '400px',
          height: '286px'
        }
      } else {
        return {
          backgroundImage: `url(https://static01.nyt.com/${this.thumbImage.content})`,
          backgroundPosition: 'center',
          backgroundRepeat: 'no-repeat',
          width: '75px',
          height: '75px'
        }
      }
    }
  }
}
</script>

<style lang="scss">
  .article-list-item {
    padding: 30px 12px 30px 12px;
    border-bottom: 1px solid #e2e2e2;
    .headline {
      .image {
        display: inline-block;
        margin-right: 10px;
      }
      margin: 0 auto 6px;
      text-align: left;
      font-size: 1.15rem;
      line-height: 1.25rem;
      font-weight: 700;
      span {
        display: inline-block;
        width: calc(90vw - 90px);
        vertical-align: top;
      }
    }
    .byline {
      text-align: left;
      margin: 0 auto 16px;
      text-transform: uppercase;
      font-size: 12px;
      font-weight: 100;
    }
    .snippet {
      margin: 0 auto;
      text-align: left;
      font-size: 14px;
      font-family: Georgia, serif;
      span.text::after {
        content: " "
      }
    }
  }

  @media (min-width: 450px) {
    .article-list-item {
      width: 50vw;
      .headline {
        .image {
          min-width: 100%;
          margin-bottom: 16px;
        }
        width: 75%;
        padding-bottom: 12px;
        font-size: 1.5625rem;
        line-height: 1.6875rem;
        font-weight: 700;
        span {
          display: inline;
          width: 100%;
          margin-left: 0;
          position: inherit;
          top: 0;
        }
      }
      .byline {
        width: 75%;
      }
      .snippet {
        width: 75%;
        font-size: 16px;
      }
    }
  }
</style>
