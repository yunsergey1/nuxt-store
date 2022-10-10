<template>
    <section>
        <h1 class="text-uppercase bg-warning bg-gradient">All products page</h1>
        <div class="album py-5 bg-light">
    <div class="container">
        <table class="table">
        <thead>
            <tr>
            <th scope="col">#</th>
            <th scope="col">Title</th>
            <th scope="col">Price</th>
            <th scope="col">Rating</th>
            <th scope="col">Stock</th>
            </tr>
        </thead>
        <tbody v-for="(product, idx) of products" :key="idx">
            <tr>
            <th scope="row">{{ product.id }}</th>
            <td> <a href="#" @click.prevent="openProduct(idx)"> {{ product.title }} </a> </td>
            <td>{{ product.price }}</td>
            <td>{{ product.rating }}</td>
            <td>{{ product.stock }}</td>
            </tr>
        </tbody>
        </table>
 
    </div>
  </div>
    </section>
</template>

<script>
    export default {
        data() {
            return {
                products: []
            }
        },
        // async fetch() {
        //     this.products = await this.$http.$get('https://dummyjson.com/products')
        // },
        async asyncData({ $axios }) {
            const response = await $axios.$get('https://dummyjson.com/products?limit=100')
            const products = await response.products
            return { products }
        },
        methods: {
            openProduct(idx) {
                idx += 1
                this.$router.push('/products/' + idx)
            }
        }
    }
</script>

<style scoped>
    img {
        height: 200px;
        object-fit: contain;
    }
    h4 {
        text-align: right;
        color: #dc3545
;
    }
</style>