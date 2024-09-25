<script setup>
  import Header from './components/Header.vue';
  import ProductList from './components/ProductList.vue';
  import axiosClient from './utils/axios'
  import { onMounted, ref, watch } from 'vue';

  const products = ref([]);
  const search = ref("");
  const filteredProducts = ref([]);

  const page = ref(1);
  const itemsPage = ref(9);
  const paginatedProducts = ref([]);
  const totalItems = ref(0);

  const getProducts = async () => {
    try {
      const {data} = await axiosClient.get("/products");
      products.value = data;
      totalItems.value = products.value.length;
    } catch (error) {
      console.log(error);
    }
  }

  const filterProduct = () => {
    filteredProducts.value = products.value.filter((product) => 
      product.title.toLowerCase().includes(search.value.toLowerCase())
    );
  }

  const paginaProduct = (currentProducts) => {
    const start = (page.value - 1) * itemsPage.value;
    const end = page.value * itemsPage.value;
    paginatedProducts.value = currentProducts.slice(start, end)
  }

  const changePage = (newPage) => {
    page.value = newPage;
  }


  onMounted(() => {
    getProducts();  
  });

  watch([products, page, filteredProducts], () => {
    paginaProduct(
      filteredProducts.value.length <= 0 && search.value === ""
        ? products.value
        : filteredProducts.value
    );
  });

</script>

<template>
  <Header/>

  <div class="container max-w-screen-lg mx-auto px-6">

    <div class="mb-8">
      <input 
        type="text" 
        class="border border-gray-400 rounded w-full p-1 px-4" 
        placeholder="Ingrese título del producto"
        v-model="search"
        @input="filterProduct"
      />
    </div>

    <div class="mb-8 flex justify-center space-x-6">
      <button 
        :disabled="page <= 1"
        :class="{'opacity-50': page <=1}"
        @click="$event => changePage(page - 1)"
        class="border border-gray-300 rounded px-2 py-0.5 hover:bg-gray-200"
        >
          anterior
      </button>
      <button 
        :disabled="page >= totalItems / itemsPage"
        :class="{'opacity-50': page >= totalItems / itemsPage}"
        @click="$event => changePage(page + 1)"
        class="border border-gray-300 rounded px-2 py-0.5 hover:bg-gray-200"
        >
          ver mas
        </button>
    </div>

    <!-- <ProductList :products="filteredProducts.length > 0 ? filteredProducts : products"/> -->
    <ProductList :products="paginatedProducts"/>
  </div>
  <!-- <div v-for="product in products"> {{ product.title }} </div> -->
</template>

