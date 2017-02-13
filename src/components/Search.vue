<template>
  <div id="search" class="search-bar-overlay">
    <input v-model="input">
    <p>{{ message }}</p>
  </div>
</template>
<script>
  import * as searchServices from '../services/search';

  require('es6-promise').polyfill();

  require('isomorphic-fetch');

  export default {
    name: 'Search',
    data() {
      return {
        message: 'Test!',
      };
    },
    mounted() {
      fetch('//offline-news-api.herokuapp.com/stories')
        .then((response) => {
          if (response.status >= 400) {
            throw new Error('Bad response from server');
          }
          return response.json();
        })
        .then((stories) => {
          this.message = stories;
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
