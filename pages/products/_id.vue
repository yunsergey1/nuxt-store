<template>
<div>
    <div class="card-body">
        <h1 class="">
            {{ product.title }}
        </h1>
        <p class="">
            <img v-bind:src='product.thumbnail' class="card-img-top" v-bind:alt="product.title">
        </p>
        <p class="card-text">
            <strong>Brand:</strong>  {{ product.brand }}
        </p>
        <p class="card-text">
            <strong>Category:</strong>  {{ product.category }}
        </p>
        <p class="card-text">
            <strong>Descriptions:</strong>  {{ product.description }}
        </p>
        <p class="card-text">
            <strong>Rating:</strong>  {{ product.rating }}
        </p>
        <p class="card-text">
            <strong>Stock:</strong>  {{ product.stock }} items
        </p>
        <p class="card-text">
            <strong>Discount:</strong>  {{ product.discountPercentage }} %
        </p>
        <h2 class="">
            Price: {{ product.price }} $
        </h2>
        <p class="text-end px-4">
            <a href="#" @click.prevent="goBack()">
                Go back
            </a>
        </p>
    </div>

</div>
</template>

<script>
export default {
    validate({ params }) {
        // Must be a number
        return /^\d+$/.test(params.id)
    },
    data: () => ({
        product: [],
    }),
    async asyncData({ $axios, params }) {
        const product = await $axios.$get('https://dummyjson.com/products/' + params.id)
        return { product }
    },
    methods: {
        goBack() {
            this.$router.go(-1)
        }
    }
}
</script>

<style scoped>
    img {
        max-height: 40vh;
        max-width: 100%;
        object-fit: contain;
    }
    h2 {
        margin-bottom: 1rem;
    }
    .card-body {
        max-width: 576px;
        margin: 2rem auto;
    }
</style>