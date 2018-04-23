<template lang="html">
  <div class="article">
    <div class="headline">
      <div class="image" v-if="hasMultimedia" v-bind:style="imageStyle" >
      </div>
      <span v-html="this.activeArticle.headline" ></span>
    </div>
    <div class="byline">
      {{this.activeArticle.byline}}
    </div>
    <div class="snippet">
      <span class="text" v-html="this.activeArticle.summary" ></span><span v-if="this.activeArticle.summary.length > 0"><a :href="this.activeArticle.url" >Read more...</a></span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["article", "language", "index"],
  watch: {

  },
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
    mediumImage: function() {
      return this.activeArticle.images[0].types.find(function(element){
        return element.type === "sfSpan"
      });
    },
    thumbImage: function() {
      return this.activeArticle.images[0].types.find(function(element){
        return element.type === "thumbStandard"
      });
    },
    imageStyle: function() {
      if (window.innerWidth >= 800) {
        return {
          backgroundImage: `url(https://static01.nyt.com/${this.largeImage.content})`,
          backgroundPosition: 'center',
          backgroundRepeat: 'no-repeat',
          width: '400px',
          height: '286px'
        }
      } else if (window.innerWidth >= 600 && window.innerWidth < 800) {
        return {
          backgroundImage: `url(https://static01.nyt.com/${this.mediumImage.content})`,
          backgroundPosition: 'center',
          backgroundRepeat: 'no-repeat',
          width: '395px',
          height: '263px'
        }
      }
      else {
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
  .article {
    background-color: white;
    width: 100vw;
    height: 43vh;
    box-sizing: border-box;
    padding: 30px 0;
    .headline {
      .image {
        display: inline-block;
      }
      border-bottom: 1px solid #e2e2e2;
      margin: 0 auto 6px;
      width: 90%;
      text-align: left;
      font-size: 1.15rem;
      line-height: 1.25rem;
      font-weight: 700;
      span {
        display: inline-block;
        width: calc(90vw - 90px);
        margin-left: 10px;
        vertical-align: top;
      }
    }
    .byline {
      text-align: left;
      width: 90%;
      margin: 0 auto 16px;
      text-transform: uppercase;
      font-size: 12px;
      font-weight: 100;
    }
    .snippet {
      width: 90%;
      margin: 0 auto;
      text-align: left;
      font-size: 14px;
      font-family: Georgia, serif;
      span.text::after {
        content: " "
      }
    }
  }

  @media (min-width: 600px) {
    .article {
      height: 45vh;
      .headline, .byline, .snippet {
        width: 75vw;
      }
      .headline {
        span {
          width: 100%;
          margin-left: 0;
        }
      }
    }
  }

  @media (min-width: 800px) {
    .article {
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
