<template>
  <li>
    <a :href="page.fullurl" target="_blank">
      <div>
        <span v-if="page.thumbnail" :style="{ backgroundImage: `url('${page.thumbnail.source}')` }"></span>
        <span v-if="!page.thumbnail" class="no-image"></span>
      </div>
      <div>
        <h3>{{ page.title }}</h3>
        <p>{{ textTruncate(page.extract, 200, '...') }}</p>
      </div>
    </a>
  </li>
</template>

<script>
export default {
  name: 'item',
  props: [
    'page',
  ],
  methods: {
    textTruncate(str, length, ending) {
      if (str.length > length) {
        return str.substring(0, length - ending.length) + ending;
      }
      return str;
    },
  },
};
</script>

<style lang="scss">
</style>
