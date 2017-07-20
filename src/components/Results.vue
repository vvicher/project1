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
  props: [
    'data',
    'phrase',
  ],
};
</script>
