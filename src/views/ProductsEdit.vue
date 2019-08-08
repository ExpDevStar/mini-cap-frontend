<template>
  <div class="container home">
    <h4>Edit product</h4>
    <div>
      Name:
      <input v-model="product.name" type="text" />
      Price:
      <input v-model="product.price" type="text" />
      Image URL:
      <input v-model="product.image_url" type="text" />
      Description:
      <input v-model="product.description" type="text" />
      <button v-on:click="updateProduct(product)">Update</button>
    </div>
  </div>
</template>

<style>
img {
  width: 100%;
}
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      product: {}
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      this.product = response.data;
    });
  },
  methods: {
    updateProduct: function(inputProduct) {
      var params = {
        name: inputProduct.name,
        price: inputProduct.price,
        image_url: inputProduct.image_url,
        description: inputProduct.description
      };
      axios.patch("/api/products/" + inputProduct.id, params).then(response => {
        this.$router.push("/");
      });
    }
  }
};
</script>
