<template>
    <div class="flex items-center justify-between mb-3">
        <h1 class="text-3xl font-semibold">Products</h1>
        <button
            type="button"
            class="relative inline-flex items-center rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
        >
            Add new product
        </button>
    </div>
    <div class="bg-white p-4 rounded-lg shadow">
        <div class="flex justify-between border-b-2 pb-3">
            <div class="flex items-center">
                <span class="whitespace-nowrap mr-3">Per Page</span>
                <select
                    v-model="perPage"
                    @change="getProducts(null)"
                    class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                >
                    <option value="5">5</option>
                    <option value="10">10</option>
                    <option value="20">20</option>
                    <option value="50">50</option>
                    <option value="100">100</option>
                </select>
            </div>
            <div>
                <input
                    v-model="search"
                    @change="getProducts(null)"
                    placeholder="Type to search product"
                    class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                />
            </div>
        </div>

        <Spinner v-if="products.loading" class="mt-2" />
        <template v-else>
            <table class="table-auto w-full">
                <thead>
                    <tr>
                        <th class="border-b-2 p-2 text-left">ID</th>
                        <th class="border-b-2 p-2 text-left">Image</th>
                        <th class="border-b-2 p-2 text-left">Title</th>
                        <th class="border-b-2 p-2 text-left">Price</th>
                        <th class="border-b-2 p-2 text-left">
                            Last Updated At
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="product of products.data">
                        <td class="border-b p-2">{{ product.id }}</td>
                        <td class="border-b p-2">
                            <img
                                class="w-16"
                                :src="product.image"
                                :alt="product.title"
                            />
                        </td>
                        <td class="border-b p2">{{ product.title }}</td>
                        <td class="border-b p2">{{ product.price }}</td>
                        <td class="border-b p2">{{ product.updated_at }}</td>
                    </tr>
                </tbody>
            </table>
            <div class="flex justify-between items-center mt-5">
                <span>
                    Showing from {{ products.from }} to {{ products.to }}
                </span>
                <nav
                    v-if="products.total > products.limit"
                    class="isolate inline-flex -space-x-px rounded-md shadow-sm"
                    aria-label="Pagination"
                >
                    <a
                        v-for="(link, i) of products.links"
                        :key="i"
                        :disabled="!link.url"
                        @click="getForPage($event, link)"
                        href="#"
                        aria-current="page"
                        class="relative z-10 inline-flex items-center px-4 py-2 text-sm font-semibold focus:z-20 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
                        :class="[
                            link.active
                                ? 'bg-indigo-50 border-indigo-500 text-indigo-600'
                                : 'bg-white border-gray-300 text-gray-500 hover:bg-gray-50',
                            i === 0 ? 'rounded-l-md' : '',
                            i === products.links.length - 1
                                ? 'rounded-r-md'
                                : '',
                            !link.url ? 'bg-gray-100 text-gray-700' : '',
                        ]"
                        v-html="link.label"
                    ></a>
                </nav>
            </div>
        </template>
    </div>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";
import store from "../store/index.js";
import Spinner from "../components/core/Spinner.vue";
import { PRODUCTS_PER_PAGE } from "../constants.js";

const perPage = ref(PRODUCTS_PER_PAGE);
const search = ref("");
const products = computed(() => store.state.products);

onMounted(() => {
    getProducts();
});

function getProducts(url = null) {
    store.dispatch("getProducts", {
        url,
        search: search.value,
        perPage: perPage.value,
    });
}
function getForPage(ev, link) {
    if (!link.url || link.active) return;
    getProducts(link.url);
}
</script>

<style scoped></style>
