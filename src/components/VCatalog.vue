<template>
  <div class="flex gap-5 items-center">
    <input
      type="number"
      class="px-5 py-3 border rounded border-gray-500 outline-none"
      v-model="price"
    />
    <select
      v-model="selectedCategory"
      multiple
      class="px-2 pt-5 border rounded border-gray-500 outline-none"
    >
      <option
        v-for="category in categories"
        :key="category"
        class="text-center"
      >
        {{ category }}
      </option>
    </select>
    <input
      type="text"
      class="px-5 py-3 border rounded border-gray-500 outline-none"
      v-model="searchTerm"
      @input="searchProduct"
    />
    <ul v-if="searchResults.length > 0" class="border rounded border-gray-500">
      <li
        v-for="(result, index) in searchResults"
        :key="index"
        class="px-2 py-1 hover:bg-gray-200 cursor-pointer"
        @click="selectProduct(result)"
      >
        {{ result.title }}
      </li>
    </ul>

    <button class="border border-gray-500 rounded px-5 py-2" @click="filterOk">
      Ok
    </button>
    <button
      class="border border-gray-500 rounded px-5 py-2"
      @click="resetFilters"
    >
      Сбросить
    </button>
  </div>
  <div class="py-5 grid grid-cols-4 gap-3">
    <div
      v-if="searchTerm"
      class="flex flex-col items-center justify-between border-2 border-solid border-blue-200 py-5"
    >
      <img
        :src="searchedItem.image"
        :alt="searchedItem.title"
        class="mb-5 max-w-[100px]"
      />
      <span class="text-center px-3">{{ searchedItem.title }}</span>
      <span class="text-center"
        ><span class="font-semibold">Price:</span>
        {{ searchedItem.price }}</span
      >
      <button
        class="text-base px-5 py-2 border border-solid border-black rounded-md"
        @click="showSearchedProdact"
      >
        Add to cart
      </button>
    </div>
    <v-catalog-item
      v-for="product in filteredProducts"
      :key="product.id"
      :product="product"
      @showProdact="showProdact"
    />
  </div>
</template>

<script>
import VCatalogItem from "./VCatalogItem.vue";

export default {
  components: {
    VCatalogItem,
  },
  data() {
    return {
      products: [],
      filteredProducts: [],
      basket: [],
      price: 0,
      selectedCategory: [],
      categories: [],
      searchTerm: "",
      searchResults: [],
      searchedItem: [],
    };
  },
  mounted() {
    fetch("https://fakestoreapi.com/products?limit=12")
      .then((res) => res.json())
      .then((json) => {
        this.products = json;
        this.filteredProducts = [...this.products];
        this.categories = [...new Set(json.map((product) => product.category))];
      });
  },
  methods: {
    showSearchedProdact() {
      this.basket.push(this.searchedItem);
    },
    showProdact(data) {
      this.basket.push(this.products[data - 1]);
      this.$emit("showProdact", this.basket);
    },
    filterOk() {
      if (this.price <= 0 && this.selectedCategory.length === 0) {
        return;
      }

      this.filteredProducts = this.products.filter((product) => {
        if (this.price > 0 && this.selectedCategory.length > 0) {
          if (
            product.price <= this.price &&
            this.selectedCategory.includes(product.category)
          ) {
            return true;
          }
        } else if (this.price > 0) {
          if (product.price <= this.price) {
            return true;
          } else {
            return false;
          }
        } else if (this.selectedCategory.length > 0) {
          if (this.selectedCategory.includes(product.category)) {
            return true;
          } else {
            return false;
          }
        }

        return false;
      });
    },

    resetFilters() {
      this.filteredProducts = [...this.products];
      this.selectedCategory = [];
      this.price = 0;
      this.searchTerm = "";
      this.searchResults = [];
    },

    searchProduct() {
      if (this.searchTerm === "") {
        this.searchResults = [];
        return;
      }

      const lowerCaseSearchTerm = this.searchTerm.toLowerCase();

      this.searchResults = this.products.filter((product) =>
        product.title.toLowerCase().includes(lowerCaseSearchTerm)
      );
    },

    selectProduct(product) {
      this.searchTerm = product.title;
      this.searchResults = [];
      this.searchedItem = product;
      console.log(this.searchedItem);
    },
  },
};
</script>

<style></style>
