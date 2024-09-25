<script setup>
    import { defineProps } from 'vue';

    const props = defineProps({
        product: Object
    });

</script>

<template>
    <div>
        <img :src="product.image" class="w-full h-48 object-contain">
    </div>
    <h4 class="text-xl font-bold text-gray-500">{{ product.title }}</h4>
    <h4 class="text-xl font-bold">${{ product.price }}</h4>
    <h4 class="text-xl">{{ product.description }}</h4>
    <h4 class="text-xl font-bold">id: {{ product.id }}</h4>
</template>