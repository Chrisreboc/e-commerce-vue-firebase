<script setup>
import HeaderComp from '@/components/layouts/HeaderComp.vue';
import { onMounted, ref, computed, nextTick } from 'vue'
import Swal from 'sweetalert2'


import { useProductsStore } from '@/stores/products.store';
document.title = "CRUD Products"

const productsStore = useProductsStore();

const name = ref();
const image = ref("https://placehold.co/300x200.png");
const price = ref();
const category = ref();
const description = ref();
const idProduct = ref();

const formRef = ref(null)

//ESTADO DE EDICIÓN

const editState = ref(false);
const loading = ref(false);

//COMPUTED

const validForm = computed(() => {
    let rulesForm = name.value && image.value && price.value && category.value && price.value > 0 && description.value;
    return rulesForm;
});

//ACTIONS
const resetForm = () => {
    name.value = ""
    image.value = "https://placehold.co/300x200.png";
    price.value = 1;
    category.value = "";
    description.value = "";
    idProduct.value = "";
}



const addProduct = async () => {
    loading.value = true;
    let respuesta = await productsStore.addProduct(name.value, image.value, price.value, category.value, description.value);

    loading.value = false;



    if (respuesta.success) {

        Swal.fire({
            position: "center",
            icon: "success",
            title: respuesta.success,
            showConfirmButton: false,
            timer: 2000
        });

        resetForm();
    } else {
        Swal.fire({
            icon: "error",
            title: "Error",
            text: respuesta.error,
            footer: '<a href="#">Why do I have this issue?</a>'
        });
    }
};

const editProduct = async () => {

    Swal.fire({
        title: `¿Estás seguro que deseas editar el producto?`,
        text: "Si aceptas el nombre será cambiado",
        icon: "warning",
        showCancelButton: true,
        confirmButtonText: "Si, editar!"
    }).then(async (result) => {

        if (result.isConfirmed) {

            let respuesta = await productsStore.editProduct(
                name.value,
                image.value,
                price.value,
                category.value,
                description.value,
                idProduct.value
            );

            if (respuesta.success) {
                Swal.fire("¡Editado correctamente!", "", "success");
                resetForm();
                editState.value = false;
            } else {
                Swal.fire("Error", respuesta.error, "error");
            }
        }
    });
};

const createOrEdit = () => {
    if (editState.value) {
        editProduct();
    } else {
        addProduct();
    }
}

const deleteProduct = async (id, name) => {

    Swal.fire({
        title: `Estás seguro que deseas eliminar el producto: ${name}?`,
        text: "La eliminación no se puede revertir!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Si, eliminar!"
    }).then(async (result) => {
        if (result.isConfirmed) {
            let respuesta = await productsStore.deleteProduct(id, name);
            Swal.fire({
                title: "Eliminado",
                text: respuesta.success,
                icon: "success"
            });
        }
    })
        .catch((error) => {
            alert(error);
        })
};

const preEditProduct = async (id) => {
    const product = productsStore.findProduct(id);

    name.value = product.name;
    image.value = product.image;
    price.value = product.price;
    category.value = product.category;
    description.value = product.description;
    idProduct.value = product.id;

    editState.value = true;

    // Esperar a que Vue termine de actualizar el DOM
    await nextTick();

    formRef.value?.scrollIntoView({
        behavior: 'smooth',
        block: 'start'
    });
};

const cancelEdit = () => {
    editState.value = false;
    resetForm();
}

onMounted(async () => {
    //AQUÍ PODEMOS HACER LLAMADOS A LAS APIS Y CARGARLAS EN EL DOM CUANDO SEA NECESARIO
    await productsStore.fetchProducts();
});

</script>

<template>
    <div>
        <HeaderComp>CRUD DE PRODUCTOS</HeaderComp>
    </div>

    <main class="container">

        <div class="row justify-content-center">
            <div class="col-12 col-sm-7 col-md-6 col-lg-5 form-style mb-4">
                <form @submit.prevent="createOrEdit" ref="formRef">
                    <div>
                        <input type="hidden" class="form-control" v-model="idProduct">
                    </div>
                    <div class="mb-3">
                        <label for="name.p" class="form-label">Nombre: </label>
                        <input type="text" class="form-control" required v-model="name">
                    </div>
                    <div class="mb-3">
                        <label for="name.p" class="form-label">Descripción: </label>
                        <input type="text" class="form-control" required v-model="description">
                    </div>
                    <div class="mb-3">
                        <label for="image.p" class="form-label">Imagen: </label>
                        <input type="url" class="form-control" required v-model="image">
                    </div>

                    <div class="mb-3">
                        <label for="price.p" class="form-label">Precio: </label>
                        <input type="number" class="form-control" min="1" required v-model="price">
                    </div>

                    <div class="mb-3">
                        <label for="category.p" class="form-label">Categoría: </label>
                        <select name="" id="" class="form-control" required v-model="category">
                            <option value="">Elige una Categoría</option>
                            <option :value="category.name" v-for="category in productsStore.categories"
                                :key="category.id">{{ category.name }}</option>
                        </select>
                    </div>

                    <div>
                        <v-btn variant="outlined" color="primary" type="submit" :disabled="!validForm" v-if="!editState"
                            :loading>
                            Crear
                        </v-btn>
                        <button class="btn btn-warning me-2" type="submit" :disabled="!validForm"
                            v-if="editState">Editar</button>
                        <button class="btn btn-secondary" type="submit" :disabled="!validForm" v-if="editState"
                            @click="cancelEdit">Cancelar</button>
                    </div>
                </form>
            </div>
        </div>

        <div v-if="productsStore.quantityProducts">
            <h2 class="text-center">Resultados</h2>
            <table class="table">
                <thead>
                    <tr>
                        <th scope="col">N°</th>
                        <th scope="col">Nombre</th>
                        <th scope="col">Imagen</th>
                        <th scope="col">Precio</th>
                        <th scope="col">Categoría</th>
                        <th scope="col">Acciones</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(product, index) in productsStore.products" :key="product.id">
                        <th scope="row">{{ index + 1 }}</th>
                        <td>{{ product.name }}</td>
                        <td>
                            <img :src="product.image" :alt="product.description" width="80">
                        </td>
                        <td>{{ product.price }}</td>
                        <td>{{ product.category }}</td>
                        <td>
                            <button class="btn btn-warning me-2" @click="preEditProduct(product.id)">Editar</button>
                            <button class="btn btn-danger me-2"
                                @click="deleteProduct(product.id, product.name)">Eliminar</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </main>
</template>

<style scoped lang="css">
tr,
th,
td {
    align-content: center;
}

.form-style {
    border: 2px solid rgb(255, 196, 0);
    border-radius: 10px;
    padding: 20px;
}
</style>