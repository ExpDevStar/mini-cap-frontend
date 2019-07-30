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

<style></style>

<script>
export default {
  data: function() {
    return {
      message: "Buy my stuff.",
      products: [],
      currentProducts: {},
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
      console.log("Create the recipe...");
      var params = {
        name: this.name,
        price: this.price,
        image_url: this.image_url,
        description: this.description,
      };
  // } Closing method tag
};
</script>
