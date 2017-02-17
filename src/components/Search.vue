<template>
  <div id="search" class="search-bar-overlay">
    <input v-model="searchTerm">
    <ul class="list-group" id="list1">
      <li class="list-group-item" v-for="item in result">
        <div v-for="(innerItem, index) in item">
          <b> {{ index }} - </b> {{ innerItem }}
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
  import {
    generateSearchMatrikkelNummerUrl,
    generateSearchMatrikkelVegUrl,
    generateSearchMatrikkelAdresseUrl
  } from '../services/search';

  require('es6-promise').polyfill();

  require('isomorphic-fetch');

  export default {
    name: 'Search',
    data() {
      return {
        result: 'No results yet',
        searchTerm: 'Kartverksveien',
      };
    },
    watch: {
      searchTerm(val) {
        fetch(generateSearchMatrikkelNummerUrl(val))
          .then((response) => {
            if (response.status >= 400) {
              throw new Error('Bad response from server');
            }
            return response.json();
          })
          .then((result) => {
            this.result = result;
          });
      }
    },
  };

</script>
<style>
  #search {
    text-align: left;
    color: #2c3e50;
  }

</style>
