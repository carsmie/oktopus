<template>
  <div id="search" class="search-bar-overlay">
    <input v-model="searchTerm">
    <p>{{ message }}</p>
  </div>
</template>
<script>
  import { generateSearchMatrikkelNummerUrl } from '../services/search';

  require('es6-promise').polyfill();

  require('isomorphic-fetch');

  export default {
    name: 'Search',
    data() {
      return {
        message: 'No results yet',
        searchTerm: 'Kartverksveien',
      };
    },
    mounted() {
      fetch(generateSearchMatrikkelNummerUrl('Kartverksveien'))
        .then((response) => {
          if (response.status >= 400) {
            throw new Error('Bad response from server');
          }
          return response.json();
        })
        .then((result) => {
          this.message = result;
        });
    },
  };

</script>
<style>
  #search {
    text-align: left;
    color: #2c3e50;
  }

</style>
