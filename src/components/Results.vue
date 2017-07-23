<template>
  <div class="search-results">
    <h2>Wyniki wyszukiwania dla frazy "{{ currentPhrase }}"</h2>
    <ul class="results-list">
      <list-item v-for="page in pages" :page="page" :key="page.index"></list-item>
    </ul>
  </div>
</template>

<script>

import Item from '~component/Item';

export default {
  name: 'results',
  props: [
    'pages',
    'currentPhrase',
  ],
  components: {
    'list-item': Item,
  },
};
</script>

<style lang="scss">

@import 'static/scss/_params.scss';

.search-results {
  padding: 180px $gutter 0;
  transition: $anim-opacity;

  .before-search & {
    opacity: 0;
  }

  h2 {
    font-family: $font-headers;
    font-weight: 400;
    font-size: 1.5rem;
    border-bottom: 1px solid $color-border;
    line-height: 1.3;
    padding: 0.4rem 0;
    margin: 0;

    @media (min-width: $breakpoint-desktop-from) {
      font-size: 1.625em;
    }
  }

  .results-list {
    list-style: none;
    margin-top: 0;
    padding-left: 0;
  }
}

</style>
