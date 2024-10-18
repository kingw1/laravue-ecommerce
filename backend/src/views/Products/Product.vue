<template>
    <div class="flex items-center justify-between mb-3">
        <h1 class="text-3xl font-semibold">Products</h1>
        <button
            @click="showProductModal"
            type="button"
            class="relative inline-flex items-center rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
        >
            Add new product
        </button>
    </div>
    <ProductModal
        v-model="showModal"
        :product="productModel"
        @close="onModalClose"
    />
    <ProductTable @clickEdit="editProduct" />
</template>

<script setup>
import { ref } from "vue";
import ProductModal from "./ProductModal.vue";
import ProductTable from "./ProductTable.vue";
import store from "../../store";

const defaultProductData = {
    id: "",
    title: "",
    image: "",
    description: "",
    price: "",
};

const showModal = ref(false);
const productModel = ref({ ...defaultProductData });

function showProductModal() {
    showModal.value = true;
}

function onModalClose() {
    productModel.value = { ...defaultProductData };
}

function editProduct(product) {
    store.dispatch("getProduct", product.id).then(({ data }) => {
        productModel.value = data;
        showProductModal();
    });
}
</script>

<style scoped></style>
