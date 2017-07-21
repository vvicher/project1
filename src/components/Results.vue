<template>
  <div class="search-results">
    <h2>Wyniki wyszukiwania dla frazy "{{ phrase }}"</h2>
    <ul>
      <li>{{ data }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'results',
  props: [
    'data',
    'phrase',
  ],
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
