<template>
  <div class="max-w-7xl mx-auto">
    <h1 class="text-center text-3xl font-bold mb-10">Prodacts</h1>
    <v-catalog @showProdact="showProdact" class="mb-16" />
    <h2 class="text-center text-xl font-bold mb-10">Basket</h2>
    <div class="py-5 grid grid-cols-4 gap-3">
      <v-cart
        v-for="product in basketProdacts"
        :key="product.id"
        :product="product"
        @deleteProduct="deleteProduct"
      />
    </div>
  </div>
</template>

<script>
import VCatalog from "./VCatalog.vue";
import VCart from "./VCart.vue";
export default {
  components: {
    VCatalog,
    VCart,
  },
  data() {
    return {
      basketProdacts: [],
    };
  },
  methods: {
    showProdact(data) {
      this.basketProdacts = data;
    },
    deleteProduct(deleteId) {
      for (let i = 0; i < this.basketProdacts.length; i++) {
        if (this.basketProdacts[i].id === deleteId) {
          this.basketProdacts.splice(i, 1);
        }
      }
    },
  },
};
</script>

<style></style>
