<template lang="html">
  <div class="nav">
    <img id="logo" @click="handleReset" src="https://int.nyt.com/assets/blogs/latest/images/foundation/logos/nyt-logo-185x26.svg" alt="">
    <select class="language-select" name="language" @change="handleSelect" >
      <option v-for="(language, key) in languages" :key="key" :value="language">{{language}}</option>
    </select>
    <div class="archives">
      <button type="button" name="archives" @click="handleButtonClick"> <font-awesome-icon :icon="archive" /> <span class="label"> Archives</span></button>
    </div>

  </div>
</template>

<script>
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
import faArchive from '@fortawesome/fontawesome-free-solid/faArchive'

export default {
  components: {
    FontAwesomeIcon
  },
  computed: {
    archive: () => {
      return faArchive
    }
  },
  props: ["handleSelect", "handleButtonClick", 'handleReset'],
  data: function() {
    return {
      languages: [
        "English",
        'Martian',
      ]
    }
  }
}
</script>

<style lang="scss">
  @import "../assets/settings.scss";

  .nav {
    box-sizing: border-box;
    height: 58px;
    border-bottom: $gray_border;
  }
  .archives button {
    padding: 7px 10px;
    border: 1px solid $times_dark_blue;
    font-family: $font_bold;
    font-size: 1rem;
    font-weight: 700;
    background-color: $times_blue;
    color: white;
    text-transform: uppercase;
    cursor: pointer;
    transition: color .1s, background-color .1s;
    position: absolute;
    right: 27%;
    margin-top: .75rem;
    margin-bottom: .75rem;
    .label {
      display: none;
    }
    &:hover {
      color: $times_blue;
      background-color: white;
    }
  }
  #logo {
    max-height: 34px;
    max-width: 50%;
    position: absolute;
    left: 6%;
    top: 16px;
  }
  .language-select {
    appearance: none;
    background-color: transparent;
    border: $gray_border;
    font-family: Georgia, serif;
    font-size: 12px;
    font-weight: 300;
    padding-left: 7px;
    cursor: pointer;
    height: 34px;
    margin-top: .75rem;
    margin-bottom: .75rem;
    position: absolute;
    right: 5%;
    width: 20%;
    user-select: none;
    z-index: 3;
    -ms-user-select: none;
    -moz-user-select: none;
    -moz-appearance: none;
    -webkit-user-select: none;
    -webkit-appearance: none;
  }

  @media (min-width: 450px) {
    .archives button {
      right: 33%;
      .label {
        display: inline;
      }
    }
    .language-select {
      padding-left: 15px;
      width: 25%;
    }
  }
</style>
