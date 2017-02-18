<template>
  <div id="search">
    <form class="form">
      <div class="input-group">
        <span class="input-group-addon" id="basic-addon1">search for</span>
        <input class="form-control" v-model="searchTerm">
      </div>
    </form>
    <div id="result">
      <ul class="list-group" id="list1">
        <li class="list-group-item" v-for="item in result">
          <div v-for="(innerItem, index) in item">
            <b> {{ index }} - </b> {{ innerItem }}
          </div>
        </li>
      </ul>
    </div>
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
   padding: 45px 15px 15px;
}
.form {
    padding: 0px 0px 15px;
}
</style>
