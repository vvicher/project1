<template>
  <div v-bind:class="[{ loading: loading, 'before-search': beforeSearch }, 'search-page']">
    <div class="search-container">
      <h1>Wikipedia search :)</h1>
      <form class="search-form" autocomplete="off">
        <input type="text" placeholder="Wpisz czego szukasz..." v-model="phrase">
        <button @click.stop.prevent="search">Szukaj</button>
      </form>
    </div>
    <results v-if="!beforeSearch"
      :pages="data.query.pages"
      :phrase="phrase"
    ></results>
    <div v-if="errorMessage" class="message">
      <p>{{ errorMessage }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Results from '~component/Results';

export default {
  name: 'search',
  data() {
    return {
      apiUrl: 'https://pl.wikipedia.org/w/api.php',
      loading: false,
      beforeSearch: true,
      data: {},
      errors: [],
      errorMessage: '',
      phrase: '',
    };
  },
  methods: {
    search() {
      if (this.phrase.length > 0) {
        this.fetchData();
      } else {
        this.errorMessage = 'Nie wpisano żadnej frazy.';
        this.beforeSearch = true;
      }
    },
    fetchData() {
      this.loading = true;
      axios.get(`${this.apiUrl}`, {
        params: {
          origin: '*',
          action: 'query',
          format: 'json',
          formatversion: 2,
          prop: 'pageimages|extracts|info',
          exintro: '',
          explaintext: '',
          inprop: 'url',
          pithumbsize: 500,
          utf8: 1,
          generator: 'search',
          gsrsearch: this.phrase,
        },
      })
      .then((response) => {
        this.data = response.data;
        if (this.data.query.pages.length > 0) {
          this.beforeSearch = false;
          this.loading = false;
        }
        this.hideError();
      })
      .catch((e) => {
        this.errors.push(e);
        if (!this.data.query) {
          this.errorMessage = 'Niestety nic nie znaleziono, spróbuj ponownie.';
          this.beforeSearch = true;
        }
        this.loading = false;
      });
    },
    hideError() {
      if (this.errorMessage.length > 0) {
        this.errorMessage = '';
      }
    },
  },
  components: {
    results: Results,
  },
};
</script>

<style lang="scss">

@import 'static/scss/_params.scss';
@import 'static/scss/_base.scss';

.search-page {
  transition: $anim-opacity;
  height: 100%;
  box-sizing: border-box;
  position: relative;

  * {
    box-sizing: border-box;
  }

  &.loading {
    opacity: 0.4;
    transition: none;
  }

  h1 {
    font-family: $font-headers;
    font-weight: 400;
    text-align: center;
    margin: 0 0 1.5rem;
    line-height: 1.2em;
  }
}

.search-container {
  @include common-border-shadow;
  width: calc(100% - #{$gutter} * 2);
  line-height: 1;
  top: 1.25rem;
  left: 0;
  position: absolute;
  background-color: $color-white;
  padding: 1.25rem 1.25rem 1.75rem;
  margin: 0 $gutter;
  transition: $anim-transform, $anim-top;
  transform: translateY(0);

  .before-search & {
    top: 45%;
    transform: translateY(-50%);
  }
}

.message {
  padding: $gutter;

  p {
    @include common-border-shadow;
    position: relative;
    width: 100%;
    max-width: 30rem;
    background-color: $color-error;
    color: $color-white;
    font-size: 0.9375rem;
    margin: 0 auto;
    padding: 1rem 1rem 1rem 3rem;

    &:before {
      content: ':(';
      position: absolute;
      left: 1rem;
      top: 50%;
      line-height: 2rem;
      font-size: 1.5rem;
      transform: translateY(-55%);
    }
  }
}

.search-form {
  max-width: 40rem;
  margin: 0 auto;
  overflow: hidden;

  input {
    display: block;
    width: calc(100% - 85px);
    float: left;
    height: 2.125rem;
    outline: none;
    border: 1px solid $color-border;
    transition: $anim-border-color;
    padding: 0 1em;
    font-family: $font-main;
    font-size: 0.875rem;

    &:focus {
      border-color: $color-accent;
    }
  }

  button {
    display: block;
    width: 75px;
    height: 2.125rem;
    float: right;
    background-color: $color-accent;
    color: $color-white;
    font-family: $font-main;
    font-size: 0.875rem;
    text-transform: uppercase;
    padding: 0.5rem 0;
    border: 0;
    outline: none;
    transition: $anim-background-color;
    cursor: pointer;

    &:hover {
      background-color: $color-accent-dark;
    }
  }
}

</style>
