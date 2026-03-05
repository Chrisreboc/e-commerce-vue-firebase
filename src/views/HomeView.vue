<template>
    <div>

        <HeaderComp>PÁGINA PRINCIPAL</HeaderComp>

        <!-- <main class="container">
            <div class="card-quantity-products">
                <p class="card-quantity-product__value">
                    {{ productStore.quantityProducts }}
                </p>
                <p class="card-quantity-product__text">
                    Cantidad Productos
                </p>
            </div>
        </main> -->

        <main class="container">

            <SlideshowComp/>

            <section id="cocina">
                <h2 class="py-3 fs-3">PRODUCTOS DE COCINA</h2>

                <ListProducts :products="productStore.filterProductsByCategory('cocina')"/>
            </section>

            <v-divider></v-divider>

            <section id="hogar">
                <h2 class="py-3 fs-3">PRODUCTOS DE HOGAR</h2>

                <ListProducts :products="productStore.filterProductsByCategory('hogar')"/>
            </section>

            <v-divider></v-divider>

            <section id="jardin" class="mb-4">
                <h2 class="py-3 fs-3">PRODUCTOS DE JARDÍN</h2>

                <ListProducts :products="productStore.filterProductsByCategory('jardín')"/>
            </section>



        </main>



    </div>
</template>

<script setup>
import HeaderComp from '@/components/layouts/HeaderComp.vue';
import ListProducts from '@/components/ListProducts.vue';
import SlideshowComp from '@/components/SlideshowComp.vue';
import { useProductsStore } from '@/stores/products.store';
import { onMounted } from 'vue';

const productStore = useProductsStore();

onMounted(async () => {
    //AQUÍ PODEMOS HACER LLAMADOS A LAS APIS Y CARGARLAS EN EL DOM CUANDO SEA NECESARIO
    await productStore.fetchProducts();
});


</script>

<style lang="css" scoped>
.card-quantity-products {
    max-width: 300px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    border: 2px solid black;
    padding: 1rem;
    text-align: center;
}

.card-quantity-product__value {
    font-size: 4rem;
    font-weight: 700;
}

.card-quantity-product__text {
    font-size: 2rem;
    font-weight: 600;
}
</style>
