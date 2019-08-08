<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Search</p>
    <input v-model="searchFilter" type="text" list="names" />
    <datalist id="names">
      <option v-for="product in products">{{ product.name }}</option>
    </datalist>
    <button v-on:click="sortAttribute = 'name'" class="btn btn-primary">Sort by name</button>
    <button v-on:click="sortAttribute = 'price'" class="btn btn-primary">Sort by price</button>
    <div
      is="transition-group"
      class="row"
      appear
      enter-active-class="animated zoomInDown"
      leave-active-class="animated fadeOut"
    >
      <div v-for="product in orderBy(filterBy(products, searchFilter, 'name'), sortAttribute)" v-bind:key="product.id">
        <div class="card" v-bind:class="{ selected: product === currentProduct }" v-on:click="currentProduct = product">
          <h2>{{ product.name }}</h2>
          <img v-bind:src="product.image_url" alt="" />
          <p>Name: {{ product.name }}</p>
          <router-link v-bind:to="`/products/${product.id}`">More info</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
img {
  width: 50%;
}
.selected {
  color: white !important;
  background-color: steelBlue;
  transition: background-color 1s ease;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Buy my stuff.",
      products: [],
      searchFilter: "",
      sortAttribute: "name",
      currentProduct: {}
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
    }
  }
}; //Closing method tag
</script>
