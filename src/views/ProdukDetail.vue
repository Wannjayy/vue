<template>
    <div class="produk-detail">
        <Navbar />
        <div class="container">

            <div class="row mt-5">
                <div class="col">
                    <nav aria-label="breadcrumb">
                        <ol class="breadcrumb">
                            <li class="breadcrumb-item"><router-link to="/" class="text-dark">Home</router-link></li>
                            <li class="breadcrumb-item"><router-link to="/produk" class="text-dark">Produk</router-link></li>
                            <li class="breadcrumb-item active" aria-current="page">Produk Order</li>
                        </ol>
                    </nav>
                </div>
            </div>

            <div class="row">
                <div class="col md-6">
                    <!--<img :src="'../assets/images'+product.gambar" class="img-fluid"/>-->
                </div>
                <div class="col md-6">
                    <h2><strong>{{ $route.params.id }}</strong></h2>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue'
import axios from "axios"

export default {
    name: 'ProdukDetail',
    components: {
        Navbar,
    },
    data() {
        return {
            product: {}
        }
    },
    methods: {
        setProduct(data) {
            this.product = data
        }
    },
    mounted() {
        axios
            .get("https://api-wannjayy.vercel.app/allproduk/"+this.$route.params.id)
            .then((response) => this.setProducts(response.data))
            .catch((error) => console.log(error))
    }
}
</script>

<style>

</style>