<template>
  <div class="search-results">
    <h2>Wyniki wyszukiwania dla frazy "{{ phrase }}"</h2>
    <ul>
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
    'phrase',
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
    border-bottom: 1px solid $color-border;
    line-height: 1.6;
    margin: 0;
  }

  ul {
    list-style: none;
    padding-left: 0;
  }
}

</style>
