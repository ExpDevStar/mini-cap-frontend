<template>
  <div class="home">
    <h1>New Product</h1>
    <div>
      Name:
      <input v-model="name" type="text" />
    </div>
    <div>
      Price:
      <input v-model="price" type="number" />
    </div>
    <div>
      Image URL:
      <input v-model="imageUrl" type="text" />
    </div>
    <div>
      Description:
      <input v-model="description" type="text" />
    </div>
    <button v-on:click="createProduct()">Create</button>
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" alt="" />
      <div>
        <button v-on:click="showProduct(product)">More info</button>
      </div>
      <div v-if="product === currentProduct">
        <p>Price: {{ product.price }}</p>
        <p>Image: {{ product.image_url }}</p>
        <p>Description: {{ product.description }}</p>
        <h4>Edit Product</h4>
        <div>
          Name:
          <input v-model="product.name" type="text" />
        </div>
        <div>
          Price:
          <input v-model="product.price" type="number" />
        </div>
        <div>
          Image URL:
          <input v-model="product.image_url" type="text" />
        </div>
        <button v-on:click="updateProduct(product)">Update</button>
      </div>
    </div>
  </div>
</template>

<style>
img {
  width: 50%;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Buy my stuff.",
      products: [],
      currentProduct: {},
      name: "",
      price: "",
      imageUrl: "",
      description: ""
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
        name: this.name,
        price: this.price,
        image_url: this.image_url,
        description: this.description
      };
      axios
        .post("/api/products", params)
        .then(response => {
          console.log("Success", response.data);
          this.products.push(response.data);
          this.name = "";
          this.price = "";
          this.imageUrl = "";
          this.description = "";
        })
        .catch(error => console.log(error.response));
    },
    showProduct: function(inputProduct) {
      if (this.currentProduct === inputProduct) {
        this.currentProduct = {};
      } else {
        this.currentProduct = inputProduct;
      }
    },
    updateProduct: function(inputProduct) {
      var params = {
        name: inputProduct.name,
        price: inputProduct.price,
        image_url: inputProduct.image_url,
        description: inputProduct.description
      };
      axios.patch("/api/products/" + inputProduct.id, params).then(response => {
        console.log("Update successful", response.data);
        inputProduct.name = response.data.name;
        inputProduct.price = response.data.price;
        inputProduct.imageUrl = response.data.imageUrl;
        inputProduct.description = response.data.description;
      });
    },
    destroyProduct: function(inputProduct) {
      axios.delete("/api/products/" + inputProduct.id).then(response => {
        console.log("Delete successful", response.data);
        var index = this.products.indexOf(inputProduct);
        this.products.splice(index, 1);
      });
    }
  } //Closing method tag
};
</script>
