<template>
  <div v-bind:class="[{ loading: loading, 'before-search': beforeSearch }, 'search-page']">
    <div class="search-container">
      <h1>Wikipedia search :)</h1>
      <form class="search-form" autocomplete="off">
        <input type="text" placeholder="Wpisz czego szukasz..." v-model="phrase">
        <button @click.stop.prevent="fetchData">Szukaj</button>
      </form>
    </div>
    <results v-if="!beforeSearch"
      :data="data"
      :phrase="phrase"
    ></results>
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
      data: [],
      errors: [],
      phrase: '',
    };
  },
  computed: {
  },
  methods: {
    fetchData() {
      this.loading = true;
      axios.get(`${this.apiUrl}?origin=*&action=query&generator=search&utf8=1&format=json&gsrsearch=${this.phrase}`)
      .then((response) => {
        this.data = response.data;
        const keys = Object.keys(this.data.query.pages);
        if (keys.length) {
          this.beforeSearch = false; // TODO
        }
        this.loading = false;
      })
      .catch((e) => {
        this.errors.push(e);
        this.beforeSearch = false;
        this.loading = false;
      });
    },
  },
  components: {
    results: Results,
  },
};
</script>

<style lang="scss">

@import 'static/scss/_base.scss';

.search-page {
  transition: $anim-opacity;
  padding: 0.9375rem;
  height: 100%;
  box-sizing: border-box;

  &.loading {
    opacity: 0.4;
    transition: none;
  }

  h1, h2 {
    font-family: $fontHeaders;
    font-weight: 400;
  }

  h1 {
    text-align: center;
    margin: 0 0 1.5rem;
  }

  h2 {
    border-bottom: 1px solid $color-border;
    line-height: 1.6;
  }
}

.search-container {
  background-color: $color-white;
  box-shadow: 0 1px 5px rgba(0,0,0,0.1);
  border-radius: 3px;
  padding: 1.25rem;
  transition: $anim-transform;

  .before-search & {
    transform: translateY(30vh);
  }
}

</style>
