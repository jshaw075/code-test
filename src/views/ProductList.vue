<template>
  <div class="product-list-container">
    <label for="selector">
      Filter:
      <select v-model="select" id="selector">
        <option value="all" selected>All</option>
        <option value="purchased">Purchased</option>
        <option value="unpurchased">Unpurchased</option>
        <option value="onetime">One time purchases</option>
        <option value="recurring">Subscriptions</option>
      </select>
    </label>
    <h1>SELECTED FILTER: {{ select }}</h1>
    <div class="products-container">
      <div v-for="(product, idx) in products" :key="idx" class="product-container">
        <product
          :id="product.id"
          :title="product.title"
          :image="product.image"
          :description="product.description"
          :price="product.price"
          :purchased="product.purchased"
          :cycle="product.cycle"
          :type="product.type"
          :duration="product.duration"
          :discount="product.discount"
          :order="product.order"
        ></product>
      </div>
    </div>
  </div>
</template>

<script>
import Product from "../components/Product.vue";

const productItems = require("@/assets/products.json");

export default {
  components: { Product },
  name: "ProductList",
  computed: {
    products() {
      if (this.select == "all") {
        return productItems;
      }

      if (this.select == "purchased") {
        return productItems.filter(product => {
          return product.purchased === true
        })
      }

      if (this.select == "unpurchased") {
        return productItems.filter(product => {
          return product.purchased === false
        })
      }

      if (this.select == "onetime") {
        return productItems.filter(product => {
          return product.type === "onetime"
        })
      }

      if (this.select == "recurring") {
        return productItems.filter(product => {
          return product.type === "recurring"
        })
      }

      return productItems;
    }
  },
  data() {
    return {
      select: "all"
    };
  }
};
</script>

<style lang="scss">
.product-list-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.products-container {
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  align-items: stretch;
  max-width: 1600px;
  width: 100%;
  @media (min-width: 600px) {
    justify-content: unset;
  }
  .product-container {
    margin: 1%;
    /**
     * 2 columns
     */
    @media (min-width: 600px) {
      flex: 0 1 48%;
    }
    /**
     * 3 columns
     */
    @media (min-width: 801px) {
      flex: 0 1 31%;
    }
    /**
     * 4 columns
     */
    @media (min-width: 1025px) {
      flex: 0 1 23%;
    }
  }
}
</style>
