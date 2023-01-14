<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs" />
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
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Produk</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <th>{{ index + 1 }}</th>
                  <td>
                    <img
                      :src="'../assets/images/' + keranjang.product.gambar"
                      class="img-fluid shadow"
                      width="250"
                    />
                  </td>
                  <td>
                    <strong>{{ keranjang.product.nama }}</strong>
                  </td>
                  <td>{{ keranjang.keterangan }}</td>
                  <td align="center">{{ keranjang.jumlah_pesanan }}</td>
                  <td align="right">Rp. {{ keranjang.product.harga }}</td>
                  <td align="right">
                    <strong
                      >Rp.
                      {{
                        keranjang.product.harga * keranjang.jumlah_pesanan
                      }}</strong
                    >
                  </td>
                  <td align="center" class="text-danger">
                    <b-icon-trash
                      @click="hapusKeranjang(keranjang._id)"
                    ></b-icon-trash>
                  </td>
                </tr>

                <tr>
                  <td colspan="6" align="right">
                    <strong>Total Harga :</strong>
                  </td>
                  <td align="right">Rp. {{ totalHarga }}</td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <div class="row justify-content-end">
        <div class="col-md-4">
          <form class="mt-4" v-on:submit.prevent>
            <div class="col md-6">
              <div class="form-group">
                <label for="nama">Nama :</label>
                <input type="text" class="form-control" v-model="pesan.nama" />
              </div>
              <div class="form-group">
                <label for="alamat">Alamat :</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="pesan.alamat"
                />
              </div>
              <button
                type="submit"
                class="btn btn-success float-right"
                @click="checkout"
              >
                <b-icon-cart></b-icon-cart>Beli
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
      keranjangs: [],
      pesan: {},
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(_id) {
      axios
        .delete("https://wannjay-api-tokopaedi.vercel.app/keranjang/" + _id)
        .then(() => {
          this.$toast.error("Produk telah dihapus dari keranjang !", {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
          axios
            .get("https://wannjay-api-tokopaedi.vercel.app/keranjang")
            .then((response) => this.setKeranjangs(response.data))
            .catch((error) => console.log(error));
        })
        .catch((error) => console.log(error));
    },
    checkout() {
      if (this.pesan.nama && this.pesan.alamat) {
        this.pesan.keranjangs = this.keranjangs;
        axios
          .post("https://wannjay-api-tokopaedi.vercel.app/pesanan", this.pesan)
          .then(() => {

            this.keranjangs.map(function (item) {
              return axios
                .delete("https://wannjay-api-tokopaedi.vercel.app/keranjang/" + item._id)
                .catch((error) => console.log(error));
            });

            this.$router.push({ path: "/pesanan-sukses" });
            this.$toast.success("Sukses Dipesan", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err));
      } else {
        this.$toast.error("Nama dan Alamat harus diisi !", {
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
      .get("https://wannjay-api-tokopaedi.vercel.app/keranjang")
      .then((response) => this.setKeranjangs(response.data))
      .catch((error) => console.log(error));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.product.harga * data.jumlah_pesanan;
      }, 0);
    },
  },
};
</script>

<style>
</style>