<template>
  <div class="container new">
    <h1>New Recipe</h1>
    Title:
    <input v-model="title" type="text" />
    Chef:
    <input v-model="chef" type="text" />
    Prep time:
    <input v-model="prepTime" type="text" />
    Ingredients:
    <input v-model="ingredients" type="text" />
    Directions:
    <input v-model="directions" type="text" />
    Image url:
    <input v-model="imageUrl" type="text" />
    <button v-on:click="createRecipe()">Create</button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      title: "",
      chef: "",
      prepTime: "",
      ingredients: "",
      directions: "",
      imageUrl: ""
    };
  },
  methods: {
    createRecipe: function() {
      console.log("Create the recipe...");
      var params = {
        title: this.title,
        chef: this.chef,
        prep_time: this.prepTime,
        ingredients: this.ingredients,
        directions: this.directions,
        image_url: this.imageUrl
      };
      axios
        .post("/api/recipes", params)
        .then(response => {
          console.log("Success", response.data);
          this.$router.push("/");
        })
        .catch(error => console.log(error.response));
    }
  }
};
</script>
