<template>
  <div class="product">
    <img class="product-image" :src="image"/>
    <div class="tab">
      <div class="left">
        <div class="title">
          {{ title }}
        </div>
        <div class="price-row">
          <div class="title">
            {{ displayPrice(price, discount) }}
          </div>
          <div class="title discount" v-if="discount">
            {{ displayPrice(price) }}
          </div>
        </div>
        <div class="description">
          {{ description }}
        </div>
      </div>
      <div class="right">
        <img src="../assets/louisa-henry.png" alt="Female / Louisa Henry" class="circle-image"/>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Product",
  props: {
    id: Number,
    title: String,
    image: String,
    description: String,
    price: Number,
    purchased: Boolean,
    cycle: String,
    type: String,
    duration: Number,
    discount: String,
    order: Number
  },
  methods: {
    /**
     * Converts a number into a converted currency string
     * e.g. 20000 would return £20,000.00
     * 
     * @param {number} pence
     * @returns {string}
     */
    convertPriceInPenceToCurrencyString: function (pence) {
      // If no price is provided then return "n/a"
      if (pence === null) {
        return "n/a"
      }

      // Converts pence to pounds
      var pounds = pence / 100

      return pounds.toLocaleString('en-UK', {
        style: 'currency', 
        currency: 'GBP', 
        minimumFractionDigits: 2 
      });
    },
    /**
     * Returns the full price or discouted price if a discount 
     * is present
     * 
     * @param {number} pence
     * @param {string} discount
     * @returns {string}
     */
    displayPrice: function (pence, discount = null) {
      // If no discount is present then return the full price
      if (discount === null) {
        return this.convertPriceInPenceToCurrencyString(pence)
      }

      // Convert string percentage to float "35%" to 0.35
      var discountPercentage = parseFloat(discount) / 100.0;

      // Takes the discount off the full price
      var discountedPriceInPence = pence - (pence * discountPercentage)

      return this.convertPriceInPenceToCurrencyString(discountedPriceInPence)
    }
  }
};
</script>

<style lang="scss">
.product {
  border-radius: 8px;
  display: block;
  width: 100%;
  height: 100%;
  max-height: 350px;
  max-width: 465px;
  position: relative;
  overflow: hidden;
  .product-image {
    display: block;
    width: 100%;
    height: auto;
  }
  .tab {
    background-color: rgba(40, 40, 40, 0.8);
    display: flex;
    justify-content: space-between;
    bottom: 0;
    width: 100%;
    position: absolute;
    .right {
      padding: 16px;
      .circle-image {
        height: 32px;
        widows: 32px;
      }
    }
    .left {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      padding: 16px;
      text-align: left;
      .price-row {
        display: flex;
      }
      .title.discount {
        font-weight: 400;
        text-decoration: line-through;
        margin-left: 10px;
        color: darken(#ffffff, 20%);
      }
      .title {
        font-weight: 600;
        font-size: 16px;
        line-height: 19px;
        color: #ffffff;
        margin-bottom: 8px;
      }
      .description {
        font-weight: 500;
        font-size: 12px;
        line-height: 14px;
        color: #BDBDBD;
      }
    }
  }
}
</style>
