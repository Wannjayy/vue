<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>Daftar <strong>Produk</strong></h2>
        </div>
      </div>
    </div>

    <div class="row mt-3">
      <div class="col">
        <div class="input-group mb-3">
          <input
            v-model="search"
            type="text"
            class="form-control"
            placeholder="Cari Produk"
            aria-label="Cari"
            aria-describedby="basic-addon1"
            @keyup="searchProduk"
          />

          <div class="input-group-prepend">
            <span class="input-group-text" id="basic-addon1"
              ><b-icon-search></b-icon-search
            ></span>
          </div>
        </div>
      </div>
    </div>
    <div class="row mb-4">
      <div class="col-md-3 mt-4" v-for="product in products" :key="product.id">
        <CardProduct :product="product" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import CardProduct from "@/components/CardProduk.vue";
import axios from "axios";

export default {
  name: "Produk-",
  components: {
    Navbar,
    CardProduct,
  },
  data() {
    return {
      products: [],
      search: "",
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
    searchProduk() {
      axios
        .get("https://wannjay-api-tokopaedi.vercel.app/allproduk" + this.search)
        .then((response) => this.setProduct(response.data))
        .catch((error) => console.log(error));
    },
  },
  mounted() {
    axios
      .get("https://wannjay-api-tokopaedi.vercel.app/allproduk")
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>


<style>
</style>