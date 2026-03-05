<template>
    <div>

        <div class="card mx-auto w-100">
            <img :src="product.image" class="card-img-top card-img-style" alt="product.name">
            <div class="card-body">
                <h5 class="card-title">{{ product.name }}</h5>
                <p class="card-text">{{ product.description }}</p>
                <p class="card-text card-style">Precio: ${{ product.price }}</p>
            </div>
        </div>

    </div>
</template>

<script setup>
defineProps({
    product: {
        type: Object,
        required: true
    }
})

</script>


<style lang="css" scoped>
.card-img-style{
    aspect-ratio: 3/2;
}

.card-style{
    font-size: 1.5rem;
    font-weight: 600;
}

</style>