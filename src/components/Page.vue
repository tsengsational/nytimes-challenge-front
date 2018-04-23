<template lang="html">
  <div class="page">
    <div class="flip-container" :style="{zIndex: this.flippedIndex}" :class="{flip: flip}">
      <div class="flipper">
        <div class="front" @click="this.flipCard">
          <articleCard class="bottom" :article="this.front" :language="language" :index="index" />
        </div>
        <div class="back" @click="this.flipCard">
          <articleCard class="top" :article="this.back" :language="language" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ArticleCard from './Article'

export default {
  components: {
      ArticleCard
    },
  props: ['index', 'articles', 'resetPages', 'language', 'flipToIndex'],
  data: function() {
    return {
      flip: false,
      flippedIndex: 999,
    }
  },
  computed: {
    front: function () {
      return this.articles[this.index]
    },
    back: function() {
      return this.articles[this.index + 1]
    }
  },
  watch: {
    flip: function (newState, oldState) {
      if (newState === true) {
        setTimeout(() => {
          let indexViewed = this.index + 1
          this.flippedIndex = indexViewed
          console.log("flipping to page ",this.flippedIndex)
          this.$emit('pageFlip', indexViewed)
        }, 600)
      } else if (newState === false) {
          this.flippedIndex = 999 - this.index
          let indexViewed = this.index + 1
          console.log("unflipping to page ", indexViewed)
          this.$emit('pageFlip', indexViewed)
      }
    },
    resetPages: function (newState) {
      if (newState === true) {
        this.flip = false;
      }
    },
    flipToIndex: function (newIndex) {
      if (newIndex - 1 === this.index) {
        this.flip = false
      } else if (newIndex - 1 === this.index + 1) {
        this.flip = true
      } else if (newIndex - 1 > this.index + 1) {
        this.flip = true
      } else if (newIndex - 1 < this.index) {
        this.flip = false
      }
    }
  },
  methods: {
    flipCard: function() {
      this.flip = !this.flip;
    },
  },
  created: function () {
    this.flippedIndex = 999 - this.index
  }
}
</script>

<style lang="scss">
  @import "../assets/settings.scss";

  .flip-container {
    position:absolute;
    bottom: 0;
    perspective: 100vw;
    perspective-origin: center top;
    .flipper {
      transform-origin: 0% 50%;
    }
  }
  .flip-container.flip .flipper {
    transform: rotateX(180deg);
    transform-origin: 100% 50%;
  }

  .flip-container, .front, .back {
    height: 50%;
  }

  .flipper {
    transition: 0.6s;
    transform-style: preserve-3d;
    transform-origin: 100% 50%;

    position: relative;
  }

  .front, .back {
    backface-visibility: hidden;
    position: absolute;
    background-color: white;
  }

  .front {
    /* for firefox 31 */
    // transform: rotateX(0deg);
  }

  .back {
    transform: rotate3D(0, 1, 0, 180deg);
    transform-origin: center;
    .article.top {
      transform: rotate(180deg);
    }
  }

  @media (min-width: 450px) {
    .flip-container {
      position:absolute;
      right: 0;
      top: 0;
      perspective: 100vw;
      perspective-origin: left top;
      .flipper {
        transform-origin: 0% 50%;
      }
    }
    .flip-container, .front, .back {
      width: 50vw;
    }
    .flip-container.flip .flipper {
      transform: rotateY(-180deg);
      transform-origin: 0% 0%;
    }
    .front, .back {
      backface-visibility: hidden;
      position: absolute;
      background-color: white;
      height: $article_height;
    }
    .front {
      /* for firefox 31 */
      transform: rotateY(0deg);
    }
    .back {
      transform: rotateY(180deg);
      .article.top {
        transform: rotate(0deg);
      }
    }
  }
</style>
