<template>
  <div class="products-index">

    <h1 class="all">All Products</h1>
      <div>
        Search by Name: <input v-model="nameFilter">
      </div>

      <div> 
        <button class="btn btn-info m-1" v-on:click="setSortAttribute('name')">Sort by Name {{isAscending}}</button>
        <button class="btn btn-info m-1" v-on:click="setSortAttribute('price')">Sort by Price {{isAscending}}</button>
      </div>


      <transition-group appear enter-active-class="animated fadeIn" leave-active-class="animated fadeOut" class="row mt-5">
        
        <div class="col-sm-4" v-for="product in orderBy(filterBy(products, nameFilter, 'name'), sortAttribute, sortAscending)" v-bind:key="product.id">
          <img class="index-products-image" v-bind:src="product.image_url" v-bind:alt="product.name">
          <h2><router-link v-bind:to="'/products/' + product.id">{{ product.name }}</router-link></h2>
          <p>{{product.formatted.total}}</p>
        </div>
      </transition-group >
  </div>
</template>

<style>
  img.index-products-image {
    width: 250px;
    }
</style>

<script>
import Vue2Filters from "vue2-filters";
var axios = require('axios');

export default {
  data: function() {
    return {
      products: [],
      nameFilter: "",
      sortAttribute: "name",
      sortAscending: 1
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
  methods: {
    setSortAttribute: function(inputAttribute) {
      if (this.sortAttribute === inputAttribute) {
        this.sortAscending *= -1;
      } else {
        this.sortAscending = 1;
      }
      
      this.sortAttribute = inputAttribute;
    }
  },
  isAscending: function(inputAttribute) {
    if (this.sortAttribute === inputAttribute) {
      return this.sortAscending === 1 ? "^" : "v";
    }    
  },
  mixins: [Vue2Filters.mixin]
};
</script>