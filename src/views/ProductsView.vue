<template>
  <div>

    <HeaderComp>NUESTROS PRODUCTOS</HeaderComp>

    <main class="container">

      <div class="d-flex">
        <p class="pe-3">
          <i class="bi bi-filter"></i>
          Filtrar por categoría:
          <select v-model="filterCategory" class="select-styles">
            <option value="">Todas las categorías</option>
            <option :value="category.name" v-for="category in productsStore.categories" :key="category.id"> {{
              category.name }}</option>
          </select>
        </p>

        <div>
          
          <p>
            <i class="bi bi-search"></i>
            Buscar:
            <input type="text" v-model="filterName" class="select-styles" placeholder="Buscar por nombre..." />
          </p>
        </div>

      </div>

      <p>Cantidad de productos encontrados: {{ quantityProducts }}</p>

      <v-divider></v-divider>

      <section>

        <div class="row justify-content-evenly g-3">
          <div class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="product in filterProducts" :key="product.id">
            <ProductCard :product />
          </div>
        </div>

      </section>
    </main>

  </div>
</template>

<script setup>
import HeaderComp from '@/components/layouts/HeaderComp.vue';
import { computed, onMounted, ref } from 'vue'
import ProductCard from '@/components/ProductCard.vue';

import { useProductsStore } from '@/stores/products.store';

const productsStore = useProductsStore();

const products = ref([]);

const filterCategory = ref("");
const filterName = ref("");

const quantityProducts = ref(0);

const filterProducts = computed(() => {
  let productsFiltered = products.value;

  if (filterCategory.value) {
    productsFiltered = productsFiltered.filter(p => p.category == filterCategory.value);
  }

  if (filterName.value) {
    let name = filterName.value.toLowerCase();
    productsFiltered = productsFiltered.filter(p => p.name.toLowerCase().includes(name));
  }
  quantityProducts.value = productsFiltered.length;
  return productsFiltered;
});




onMounted(async () => {
  //AQUÍ PODEMOS HACER LLAMADOS A LAS APIS Y CARGARLAS EN EL DOM CUANDO SEA NECESARIO
  await productsStore.fetchProducts();
  products.value = productsStore.products;
});


</script>

<style lang="css" scoped>
.card-img-top {
  aspect-ratio: 3/2;
}

.select-styles {
  padding: 0.5rem;
  border-radius: 0.25rem;
  border: 1px solid #ccc;
  font-size: 1rem;
}
</style>