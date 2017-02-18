<template>
  <div id="search">
    <form class="form">
      <div class="input-group">
        <span class="input-group-addon" id="basic-addon1">search for</span>
        <input class="form-control" v-model="searchTerm">
      </div>
    </form>
    <ul class="list-group" id="list1">
      <li class="list-group-item" v-for="(searchresult, searchName) in result">
        {{ searchName }}
        <ul class="list-group" id="list2">
          <li class="list-group-item" v-for="item in searchresult">
            <div v-for="(innerItem, index) in item">
              <b> {{ index }} - </b> {{ innerItem }}
            </div>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>
<script>
  import {
    searchServices
  } from '../services/search';

  require('es6-promise').polyfill();

  require('isomorphic-fetch');

  const searchcopy = Object.assign({}, searchServices);

  export default {
    name: 'Search',
    data() {
      return {
        result: searchcopy,
        searchTerm: 'Kartverksveien',
      };
    },
    watch: {
      searchTerm(val) {
        Object.entries(searchServices).forEach(([key, searchFnc]) => {
          fetch(searchFnc(val))
            .then((response) => {
              if (response.status >= 400) {
                throw new Error('Bad response from server');
              }
              return response.json();
            })
            .then((result) => {
              this.result[key] = result;
            });
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
