<template>
  <li class="results-list-item">
    <a class="item-link" :href="page.fullurl" target="_blank">
      <div class="item-image">
        <span :style="{ backgroundImage: `url('${imageUrl}')` }"></span>
      </div>
      <div class="item-details">
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
  computed: {
    imageUrl() {
      const url = (this.page.thumbnail ? this.page.thumbnail.source : 'static/images/placeholder.svg');
      return url;
    },
  },
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

@import 'static/scss/_params.scss';

.results-list-item {
  padding: 1rem 0;
  border-top: 1px solid lighten($color-border, 20%);

  &:first-child {
    border-top: 0;
  }
}

.item-image {
  position: relative;
  padding-top: 60%;

  @media (min-width: $breakpoint-tablet-from) {
    position: static;
    padding-top: 0;
  }

  span {
    @include common-border-shadow;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    background-color: $color-white;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    @media (min-width: $breakpoint-tablet-from) {
      display: block;
      position: relative;
      float: left;
      width: 9.375rem;
      height: 6.25rem;
    }

    @media (min-width: $breakpoint-desktop-from) {
      width: 11.25rem;
      height: 7.5rem;
    }

    &:after {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      border-radius: 3px;
      background-color: rgba($color-white, 0);
      transition: $anim-background-color;
    }
  }
}


.item-link {
  color: $color-accent-dark;
  transition: $anim-color;
  text-decoration: none;
  display: block;
  overflow: hidden;

  h3 {
    margin: 0.75rem 0 0.625rem;
    font-size: 1rem;
    line-height: 1.3;

    @media (min-width: $breakpoint-desktop-from) {
      margin-top: 0;
    }

    @media (min-width: $breakpoint-desktop-from) {
      font-size: 1.125rem;
    }
  }

  p {
    margin: 0;
    color: $color-text-main;
    transition: $anim-color;
    font-size: 0.813rem;
    line-height: 1.4;

    @media (min-width: $breakpoint-desktop-from) {
      font-size: 0.875rem;
    }
  }

  &:hover {
    color: lighten($color-accent, 5%);

    p {
      color: $color-text-light;
    }

    .item-image span:after {
      background-color: rgba($color-white, 0.15);
    }
  }
}

.item-details {
  @media (min-width: $breakpoint-tablet-from) {
    width: calc(100% - 10.313rem);
    float: right;
    padding-right: 0.938rem;
  }

  @media (min-width: $breakpoint-desktop-from) {
    width: calc(100% - 12.188rem);
  }
}

</style>
