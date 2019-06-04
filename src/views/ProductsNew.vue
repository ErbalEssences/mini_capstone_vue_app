<template>
  <div class="products-new">
    <h1>New Products</h1>

    <ul>
      <li v-for="error in errors">
        {{error}}
      </li>
    </ul>


    <form v-on:submit.prevent="submit()">
      <div>
        Name: <input v-model="newProductName">
      </div>

      <div>
        Description: <input v-model="newProductDescription">
      </div>

      <div>
        Price: <input v-model="newProductPrice">
      </div>

      <div>
        Image Url: <input v-model="newProductImageUrl">
      </div>

      <input type="submit" value="Create Product">
    </form>
  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: "",   
      errors: []      

    };
  },
  created: function() { 
  },

  methods: {
    submit: function(){
      console.log("Create the product...");
      var params = {
                    name: this.newProductName,
                    description: this.newProductDescription,
                    price: this.newProductPrice,
                    image_url: this.newProductImageUrl
                    };
      axios.post("/api/products", params).then(response => {
        this.$router.push("/");
      }).catch(error => {
        console.log("it worked")
        this.errors = error.response.data.errors;
      });
    }
  }
};
</script>