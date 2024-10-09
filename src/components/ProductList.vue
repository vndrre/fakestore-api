<script>
  // Import the ChevronDown icon from lucide-vue-next
  import { ChevronDown } from 'lucide-vue-next';

  export default {
    components: {
      ChevronDown,
    },
    data() {
      return {
        products: [],
        categories: [],
        selectedCategory: 'all',
        isDropdownOpen: false, // For controlling dropdown visibility
      };
    },
    computed: {
      filteredProducts() {
        if (this.selectedCategory === 'all') {
          return this.products;
        } else {
          return this.products.filter(product => product.category === this.selectedCategory);
        }
      },
    },
    methods: {
      async fetchProducts() {
        const response = await fetch('https://fakestoreapi.com/products');
        const data = await response.json();
        this.products = data;
      },
      async fetchCategories() {
        const response = await fetch('https://fakestoreapi.com/products/categories');
        const categories = await response.json();
        this.categories = categories;
      },
      filterByCategory(category) {
        this.selectedCategory = category;
        this.isDropdownOpen = false; // Close dropdown after selecting a category
      },
      toggleDropdown() {
        this.isDropdownOpen = !this.isDropdownOpen; // Toggle dropdown visibility
      },
      addToCart(product) {
        this.$emit('add-to-cart', product);
      },
    },
    mounted() {
      this.fetchProducts();
      this.fetchCategories();
    },
  };
</script>

<template>
  <div class="container mx-auto p-10">
    <!-- Sort by Category Button -->
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold">Fake Store Products</h1>
      
      <!-- Sort by Category Dropdown -->
      <div class="relative">

        <button @click="toggleDropdown" class="flex items-center bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-700">
          <span>Sort by Category</span>
          <ChevronDown class="ml-2" /> <!-- Lucide Icon -->
        </button>

        <!-- Dropdown Menu -->
        <div v-if="isDropdownOpen" class="absolute right-0 mt-2 w-48 bg-white border rounded shadow-lg z-10">
          <button @click="filterByCategory('all')" class="block w-full text-left px-4 py-2 hover:bg-gray-200">
            All
          </button>
          
          <button 
            v-for="category in categories" 
            :key="category" 
            @click="filterByCategory(category)" 
            class="block w-full text-left px-4 py-2 hover:bg-gray-200"
          >
            {{ category }}
          </button>
          <!-- All Products Option -->

        </div>
      </div>
    </div>

    <!-- Products Grid -->
    <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-6">
      <div
        v-for="product in filteredProducts"
        :key="product.id"
        class="border p-4 rounded-md shadow-lg"
      >
        <img
          :src="product.image"
          class="w-full h-48 object-cover mb-4"
          :alt="product.title"
        />
        <h2 class="text-lg font-semibold mb-2">{{ product.title }}</h2>
        <p class="text-gray-600 mb-2">${{ product.price }}</p>
        <button
          class="bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-700"
          @click="addToCart(product)"
        >
          Add to Cart
        </button>
      </div>
    </div>
  </div>
</template>


