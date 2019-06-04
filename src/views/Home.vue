<template>
  <div class="home">
    <h1>New Products</h1>
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

    <button v-on:click="createProduct()">Create</button>

    <h1>All Products</h1>

    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <div>
        <img v-bind:src="product.image_url" alt=""> 
      </div>
      <div v-if="currentProduct === product">

        <p>Description: {{ product.description }}</p>
        <p>Total: {{ product.formatted.total }}</p>
        <h3>Edit Product</h3>
          <div>
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
            <button v-on:click= "updateProduct(product)">Update</button>
            <button v-on:click= "destroyProduct(product)">Delete</button>
          </div>
      </div>
      <button v-on:click="showProduct(product)">{{moreInfoButton}}</button>
    </div>
  </div>
</template>

<style>
img {
  width: 250px;
}

</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      products: [],
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: "",
      currentProduct: {},
      moreInfoButton: "More Info"
    };
  },
  created: function() {
  
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },

  methods: {
    createProduct: function(){
      console.log("Create the product...");
      var params = {
                    name: this.newProductName,
                    description: this.newProductDescription,
                    price: this.newProductPrice,
                    image_url: this.newProductImageUrl
                    };
      axios.post("/api/products", params).then(response => {
        console.log("Success", response.data);
        this.products.push(response.data);

        this.newProductName = "";
        this.newProductDescription = "";
        this.newProductPrice = "";
        this.newProductImageUrl = "";
      });
    },
    showProduct: function(inputProduct) {
      if (this.currentProduct === inputProduct) {
        this.currentProduct = {};
        this.moreInfoButton = "More Info";

      } else {
        this.currentProduct = inputProduct;
        this.moreInfoButton = "Show Less";
      }   
    },
    updateProduct: function(inputProduct){
      var params = {
                    name: inputProduct.name,
                    description: inputProduct.description,
                    price: inputProduct.price,
                    image_url: inputProduct.imageUrl
                    };    

      axios.patch("/api/products/" + inputProduct.id, params).then(response => {
        console.log("Success", response.data);
      });
    },
    destroyProduct: function(inputProduct){
      axios.delete("/api/products/" + inputProduct.id).then(response => {
        console.log("Success", response.data);
        var index = this.products.indexOf(inputProduct);
        this.products.splice(index, 1);
        this.moreInfoButton = "More Info";
      });      
    }
  }
};
</script>