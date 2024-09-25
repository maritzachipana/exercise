<script setup>
    import { defineProps } from 'vue';
    import ProductModal from './ProductModal.vue';

    const props = defineProps({
        products: Array
    });

</script>

<template>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3">
        <div v-for="product in props.products" class="border border-gray-500 rounded p-2">
            <ProductModal :product="product"/>
        </div>

    </div>
</template>
