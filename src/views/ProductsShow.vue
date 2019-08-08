<template>
  <div class="container show">
    <h2>{{ product.name }}</h2>
    <img v-bind:src="product.image_url" alt="" />
    <p>Price: {{ product.price }}</p>
    <p>Description: {{ product.description }}</p>
    <router-link v-bind:to="`/products/${product.id}/edit`">Edit product</router-link>
    <router-link to="/">Back to all products</router-link>
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
  methods: {}
};
</script>
