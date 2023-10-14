<template>
  <div
    class="flex flex-col items-center justify-between border border-solid border-blue-200 basis-1/4 mb-6 py-3"
  >
    <img :src="product.image" :alt="product.title" class="mb-5 max-w-[100px]" />
    <span class="text-center px-3">{{ product.title }}</span>
    <span class="text-center"
      ><span class="font-semibold">Price:</span> {{ product.price }}</span
    >
    <button
      class="text-base px-5 py-2 border border-solid border-black rounded-md"
      @click="deleteProduct"
    >
      Remove
    </button>
  </div>
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
      required: true,
    },
  },
  data() {},
  methods: {
    deleteProduct() {
      this.$emit("deleteProduct", this.product.id);
    },
  },
};
</script>

<style></style>
