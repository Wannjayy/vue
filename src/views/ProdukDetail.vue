<template>
  <div class="produk-detail">
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/produk" class="text-dark">Produk</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Produk Order
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col md-6">
          <img :src="'../assets/images/' + product.gambar" class="img-fluid shadow" />
        </div>
        <div class="col md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
          </h2>
          <h4>
            Harga : <strong>Rp.{{ product.harga }}</strong>
          </h4>
          <form class="mt-4" v-on:submit.prevent>
            <div class="col md-6">
              <div class="form-group">
                <label for="jumlah_pesanan">Jumlah Pesanan</label>
                <input
                  type="number"
                  class="form-control"
                  v-model="pesan.jumlah_pesanan"
                />
              </div>
              <div class="form-group">
                <label for="keterangan">Keterangan</label>
                <textarea
                  v-model="pesan.keterangan"
                  class="form-control"
                  placeholder=""
                ></textarea>
              </div>
              <button type="submit" class="btn btn-success" @click="pesanan">
                <b-icon-cart></b-icon-cart>Masukkan Keranjang
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "ProdukDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      product: {},
      pesan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
    pesanan() {
      if ((this.pesan.jumlah_pesanan && this.pesan.keterangan)) {
        this.pesan.product = this.product;
        axios
          .post("https://wannjay-api-tokopaedi.vercel.app/keranjang", this.pesan)
          .then(() => {
            this.$router.push({ path: "/keranjang" });
            this.$toast.success("Sukses Masuk Keranjang", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err));
      } else {
        this.$toast.error("Jumlah Pesanan dan Keterangan harus diisi !", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get("https://wannjay-api-tokopaedi.vercel.app/allproduk/" + this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
</style>