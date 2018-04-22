<template lang="html">
  <div class="nav">
    <img id="logo" @click="handleReset" src="https://int.nyt.com/assets/blogs/latest/images/foundation/logos/nyt-logo-185x26.svg" alt="">
    <select class="language-select" name="language" @change="handleSelect" >
      <option v-for="(language, key) in languages" :key="key" :value="language">{{language}}</option>
    </select>
    <div class="archives">
      <button type="button" name="archives" @click="handleButtonClick"> <font-awesome-icon :icon="archive" /> <span class="label"> Archives</span></button>
    </div>
    <div class="toggle-view" @change="handleToggle">
      <span class="icon"><font-awesome-icon :icon="list" /></span>
      <label class="switch">
        <input type="checkbox">
        <span class="slider round"></span>
      </label>
    </div>
  </div>
</template>

<script>
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
import faArchive from '@fortawesome/fontawesome-free-solid/faArchive'
import faList from '@fortawesome/fontawesome-free-solid/faList'

export default {
  components: {
    FontAwesomeIcon
  },
  computed: {
    archive: () => {
      return faArchive
    },
    list: () => {
      return faList
    }
  },
  props: ["handleSelect", "handleButtonClick", 'handleReset', 'handleToggle'],
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
  $toggle_size: 16px;

  .nav {
    position: sticky;
    top: 0;
    box-sizing: border-box;
    height: 58px;
    background-color: white;
    border-bottom: $gray_border;
    z-index: 1;
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
    max-width: 40%;
    position: absolute;
    left: 6%;
    top: 20px;
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

  .toggle-view {
    position:absolute;
    margin-top: .75rem;
    right: calc(27% + 44px );
    .icon {
      font-size: 24px;
      display: none;
      position: relative;
      top: 2.5px;
      right: 5px;
    }

    .switch {
    position: relative;
    display: inline-block;
    top: 5px;
    width: 40px;
    height: 24px;
    input {
      display: none;
    }
    .slider {
      position: absolute;
      cursor: pointer;
      // margin-top: 5px;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: #ccc;
      -webkit-transition: .4s;
      transition: .4s;
    }

    .slider:before {
      position: absolute;
      content: "";
      height: $toggle_size;
      width: $toggle_size;
      left: 4px;
      bottom: 4px;
      background-color: white;
      -webkit-transition: .4s;
      transition: .4s;
    }

    input:checked + .slider {
      background-color: $times_blue;
    }

    input:focus + .slider {
      box-shadow: 0 0 1px $times_dark_blue;
    }

    input:checked + .slider:before {
      -webkit-transform: translateX($toggle_size);
      -ms-transform: translateX($toggle_size);
      transform: translateX($toggle_size);
    }

    .slider.round {
      border-radius: 34px;
    }

    .slider.round:before {
      border-radius: 50%;
    }
  }
  }


  @media (min-width: 450px) {
    .archives button {
      right: 33%;
      .label {
        display: inline;
      }
    }
    .toggle-view {
      .icon {
        display: inline-block;
      }
      right: calc(33%  + 140px);
    }
    .language-select {
      padding-left: 15px;
      width: 25%;
    }
  }
</style>
