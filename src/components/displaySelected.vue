<template>
  <div id="selection-bar">
    <h2>Selected product</h2>
    <div class="selected-card" v-if="isSelected">
      <img :src="'/images/' + activeCard.image" alt="product-img" />
      <p>{{ activeCard.brand }}</p>
      <h4>{{ activeCard.title }}</h4>
      <span v-if="activeCard.price.finalPrice < activeCard.price.regularPrice">
        <span class="original-price" @click="isSelected = true"
          >{{ activeCard.price.regularPrice }}
          {{ activeCard.price.currency }}</span
        >
        <span class="discount-price discount-box" @click="isSelected = true"
          >{{ activeCard.price.finalPrice }}
          {{ activeCard.price.currency }}</span
        >
      </span>
      <span
        class="original-price-only original-price-box"
        v-else
        @click="isSelected = true"
        >{{ activeCard.price.regularPrice }}
        {{ activeCard.price.currency }}</span
      >
    </div>
  </div>
</template>
<script>
export default {
  name: "displaySelected",
  props: {
      products: {
          type: Array,
          required: true
      },
      isSelected: {
          type: Boolean,
      },
      activeIndex: {
          type: Number
      }
  },
  data() {
      return {

      }
  },
  computed: {
      activeCard() {
      return this.products[this.activeIndex];
    },
  }
};
</script>
