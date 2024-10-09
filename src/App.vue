<script>
  import ProductList from './components/ProductList.vue';
  import Cart from './components/Cart.vue';

  export default {
    data() {
      return {
        cart: [],
        isCartOpen: false, // State to control cart pop-up visibility
      };
    },
    methods: {
      addToCart(product) {
        this.cart.push(product);
      },
      removeFromCart(product) {
        this.cart = this.cart.filter(item => item.id !== product.id);
      },
      toggleCart() {
        this.isCartOpen = !this.isCartOpen; // Toggle cart pop-up
      }
    },
    components: {
      ProductList,
      Cart,
    }
  };
</script>

<template>
  <nav class="w-full bg-blue-500 text-white">
    <div class="text-center py-10">
      <h1 class="text-3xl font-bold">Welcome to My Fake Store!</h1>
    </div>
  </nav>

  <div class="relative">
    <!-- Cart Button in Top Right -->
    <div class="fixed bottom-5 right-3 z-50">
      <button @click="toggleCart" class="bg-blue-500 hover:bg-blue-700 text-white py-2 px-4 rounded-full relative">
        🛒 Cart ({{ cart.length }})
        <span v-if="cart.length > 0" class="absolute top-0 right-0 h-5 w-5 bg-red-500 text-xs rounded-full text-white flex items-center justify-center">
          {{ cart.length }}
        </span>
      </button>
    </div>

    <!-- Product List -->
    <ProductList @add-to-cart="addToCart" />

    <!-- Cart Pop-up -->
    <div v-if="isCartOpen" class="fixed bottom-16 right-4 w-80 bg-white border shadow-lg rounded-lg p-4 z-50">
      <Cart :cartItems="cart" @remove-from-cart="removeFromCart" />
      <button @click="toggleCart" class="mt-4 w-full bg-gray-500 hover:bg-gray-700 text-white py-2 px-4 rounded">
        Close Cart
      </button>
    </div>
  </div>
</template>

