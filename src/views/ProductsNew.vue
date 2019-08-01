<template>
  <div class="container new">
    <h1>New Product</h1>
    Name:
    <input v-model="name" type="text" />
    Price:
    <input v-model="price" type="text" />
    Image URL
    <input v-model="imageUrl" type="text" />
    Description
    <input v-model="description" type="text" />
    <button v-on:click="createProduct()">Create</button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      name: "",
      price: "",
      imageUrl: "",
      description: ""
    };
  },
  methods: {
    createProduct: function() {
      console.log("Create the product...");
      var params = {
        name: this.name,
        price: this.price,
        imageUrl: this.imageUrl,
        description: this.description
      };
      axios
        .post("/api/products", params)
        .then(response => {
          console.log("Success", response.data);
          this.$router.push("/");
        })
        .catch(error => console.log(error.response));
    }
  }
};
</script>
