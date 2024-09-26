<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';

const products = ref([])

const fetchAllProducts = async () => {
  const response = await axios.get('https://fakestoreapi.com/products')
  products.value = response.data
}

onMounted(async() => await fetchAllProducts())
</script>
<template>
  <div class="bg-slate-500 py-10 text-white">
    <h1 class="text-center font-bold text-2xl">Welcome to the fake store</h1>
  </div>

  <div class="grid grid-cols-4 gap-4">
    <article v-for="product in products" :key="product.id" >
      <img class="w-full aspect-video pbject-contain" :src="product.image">
      <h4>{{ product.title }}</h4>
    </article>
  </div>
</template>

