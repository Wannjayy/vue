<template>
    <div class="card shadow card-produk">
        <img :src=" 'assets/images/' + product.gambar " class="card-img-top" alt="...">
        <div class="card-body">
            <h5 class="card-title">{{ product.nama }}</h5>
            <p class="card-text">Harga : {{ product.harga }}</p>
            <router-link href="#" class="btn btn-success" :to="'/produk/'+product.nama"><b-icon-cart></b-icon-cart>Beli</router-link>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CardProduct',
    props: ['product']
}
</script>

<style>

</style>