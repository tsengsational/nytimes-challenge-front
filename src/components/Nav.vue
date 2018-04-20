<template lang="html">
  <div class="nav">
    <img id="logo" src="https://int.nyt.com/assets/blogs/latest/images/foundation/logos/nyt-logo-185x26.svg" alt="">
    <select class="language-select" name="language" @change="this.handleSelect" >
      <option v-for="(language, key) in languages" :key="key" :value="language">{{language}}</option>
    </select>
    <div class="archives">
      <button type="button" name="archives" @click="handleButtonClick">Get Archives</button>
    </div>

  </div>
</template>

<script>
export default {
  props: ["handleSelect", "handleButtonClick"],
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
    border: 1px solid gray;
    font-family: Georgia, serif;
    font-size: 16px;
    font-weight: 300;
    padding-left: 15px;
    cursor: pointer;
    height: 34px;
    margin-top: .75rem;
    margin-bottom: .75rem;
    position: absolute;
    right: 7%;
    width: 33%;
    user-select: none;
    z-index: 3;
    -ms-user-select: none;
    -moz-user-select: none;
    -moz-appearance: none;
    -webkit-user-select: none;
    -webkit-appearance: none;
  }

  @media (min-width: 450px) {
    #logo {

    }
    .language-select {
      width: 25%;
    }
  }
</style>
