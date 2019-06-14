<template>
  <div class="products-show">
    <h2>{{ product.name }}</h2>
      <div>
        <img class="show-product-image" v-bind:src="product.image_url" v-bind:alt="product.name">
      </div>

      <div>
        <p>Description: {{ product.description }}</p>
        <p>Total: {{ product.formatted.total }}</p>
      </div>
      <router-link v-bind:to="'/products/'+ product.id + '/update'  ">Update</router-link> 
      <div>
        <button v-on:click="destroyProduct">Delete</button>
      </div>
  </div>
</template>

<style>
img.show-product-image {
  width: 100%;
}

</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      product: {
                name: "",
                image_url: "",
                description: "",
                formatted: {
                              total: ""
                            }
                }
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id ).then(response => {
      this.product = response.data;
    });
  },
  methods: {
    destroyProduct: function(){
      axios.delete("/api/products/" + this.product.id).then(response => {
        this.$router.push("/");
      }); 
    }
  }
};
</script>
