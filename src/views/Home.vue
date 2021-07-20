<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>{{ message2 }}</h2>
    <div>
      Name:
      <input type="text" v-model="newProductParams.name" />
      Description:
      <input type="text" v-model="newProductParams.description" />
      Price:
      <input type="text" v-model="newProductParams.price" />
      Image_url:
      <input type="text" v-model="newProductParams.image_url" />
    </div>
    <button v-on:click="createProduct()">Create a new product!</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>Title: {{ product.name }}</h2>
      <p>Price: ${{ product.price }}</p>
      <img v-bind:src="product.image_url" alt="product.title" />
    </div>
  </div>
</template>
<style>
img {
  width: 400px;
}
</style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js, Marat!",
      message2: "How are things today?",
      products: [],
      newProductParams: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
        console.log("All products:", this.products);
      });
    },
    createProduct: function () {
      console.log("Creating product");
      axios
        .post("http://localhost:3000/products", this.newProductParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.products.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>
