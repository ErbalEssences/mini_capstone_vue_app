<template>
  <div class="products-edit">
    <h1>Edit Product</h1>
      <ul>
        <li v-for="error in errors">
          {{ error }}
        </li>
      </ul>
      
        <form v-on:submit.prevent="submit()">
          <div>
            Name: <input v-model="product.name">
          </div>

          <div>
            Description: <input v-model="product.description">
          </div>

          <div>
            Price: <input v-model="product.price">
          </div>

          <div>
            Image Url: <input v-model="product.image_url">
          </div>
          <input type="submit" value="Edit Product">Edit</button>
        </form>
     </div>

  </div>
</template>

<style>
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
                },
      errors: []      

    };
  },
  created: function() { 
   axios.get("/api/products/" + this.$route.params.id ).then(response => {
      this.product = response.data;
    });
  },

  methods: {
    submit: function(inputProduct){
      var params = {
                    name: this.product.name,
                    description: this.product.description,
                    price: this.product.price,
                    image_url: this.product.imageUrl
                    };    

      axios.patch("/api/products/" + this.$route.params.id, params).then(response => {
        this.$router.push("/products/" + this.$route.params.id);
      });
    }
  }
};
</script>