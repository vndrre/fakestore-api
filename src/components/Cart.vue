<script>
    export default {
    props: {
        cartItems: Array
    },
    computed: {
        total() {
        return this.cartItems.reduce((acc, item) => acc + item.price, 0).toFixed(2);
        }
    },
    methods: {
        removeFromCart(item) {
        this.$emit('remove-from-cart', item);
        }
    }
    };
</script>

<template>
    <div>
      <h1 class="text-xl font-bold mb-4">Shopping Cart</h1>
      <ul>
        <li v-for="item in cartItems" :key="item.id" class="mb-4 flex justify-between items-center">
          <span class="font-semibold">{{ item.title }}</span> - ${{ item.price }}
          <button class="bg-red-500 text-white py-1 px-2 rounded hover:bg-red-700" @click="removeFromCart(item)">
            Remove
          </button>
        </li>
      </ul>
      <p class="font-bold mt-4">Total: ${{ total }}</p>
    </div>
</template>

  