<template>
  <div class="home">
    <h1>New Product</h1>
    <button v-on:click="createProduct()">Create</button>
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" alt="" />
      <p>Price: {{ product.price }}</p>
      <p>Image: {{ product.image_url }}</p>
      <p>Description: {{ product.description }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: []
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
      console.log(this.products);
    });
  },
  methods: {
    createProduct: function() {
      console.log("Create the product...");
      var params = {
        name: "THE Example title",
        price: "Example price",
        image_url: "url",
        description: "Example description"
      };
      axios.post("/api/products", params).then(response => {
        console.log("Great Success", response.data);
        this.products.push(response.data);
      });
    }
  }
};
</script>
