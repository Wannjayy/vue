<template>
  <div class="home">
    <Navbar />
    <div class="container">
      <Hero />

      <div class="row-mt-4">
        <div class="col">
          <h2>
            Produk <strong>Terbaik</strong>
            <router-link to="/produk" class="btn btn-success float-right"
              ><b-icon-eye></b-icon-eye> Lihat Semua</router-link
            >
          </h2>
        </div>
      </div>

      <div class="row mb-4">
        <div
          class="col-md-4 mt-4"
          v-for="product in products"
          :key="product.id"
        >
          <CardProduct :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import Hero from "@/components/Hero.vue";
import CardProduct from "@/components/CardProduk.vue";
import axios from "axios";

export default {
  name: "HomeView",
  components: {
    Navbar,
    Hero,
    CardProduct,
  },
  data() {
    return {
      products: [],
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
  },
  mounted() {
    axios
      .get("https://api-wannjayy.vercel.app/produk")
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
