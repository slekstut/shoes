<template>
  <div id="selection-bar">
    <h2>Selected product</h2>
    <div class="selected-card" v-if="isSelected">
      <img :src="'/images/' + activeCard.image" alt="product-img" />
      <p>{{ activeCard.brand }}</p>
      <h4>{{ activeCard.title }}</h4>
      <span v-if="activeCard.price.finalPrice < activeCard.price.regularPrice">
        <span class="original-price" @click="isSelected = true">{{ activeCard.price.regularPrice }} {{ activeCard.price.currency }}</span>
        <span class="discount-price discount-box" @click="isSelected = true">{{ activeCard.price.finalPrice }} {{ activeCard.price.currency }}</span>
      </span>
      <span class="original-price-only original-price-box" v-else @click="isSelected = true">{{ activeCard.price.regularPrice }} {{ activeCard.price.currency }}</span>
    </div>
  </div>
</template>
<script>
export default {
  name: "displaySelected",
  props: {
    products: {
      type: Array,
      required: true,
    },
    isSelected: {
      type: Boolean,
    },
    activeIndex: {
      type: Number,
    },
  },
  data() {
    return {};
  },
  computed: {
    activeCard() {
      return this.products[this.activeIndex];
    },
  },
};
</script>

<style lang="scss">
$border-color: #d0d0d0;
$background-color2: #fafafa;
$discount-color: #ff0000;
$discount-text-color: #ffffff;
$selected-background: #e7e7e7;
$text-color: #000000;
$price-color: #777777;

#selection-bar {
  display: grid;
  // justify-items: center;
  // align-items: center;
  place-content: center;
  gap: 1em;
  // grid-template-columns: repeat(auto-fit, minmax(453px, 1fr));
  grid-template-columns: 453px;
  background: $selected-background;
  min-height: 20em;
  margin-bottom: 2em;
  h2 {
    padding: 1em 0 1em 0;
    font-style: normal;
    font-weight: 500;
    font-size: 25px;
    line-height: 1.6em;
    text-align: center;
  }
  .selected-card {
    position: relative;
    height: 170px;
    width: 100%;
    border-radius: 5px;
    background-color: $background-color2;
    border: 1px solid $border-color;
    border-radius: 5px;
    margin: 0 auto;
  }
  img {
    position: relative;
    height: 100%;
    width: 179px;
    left: 0;
    top: 0;
    border-radius: 5px 0px 0px 5px;
  }
  p {
    position: absolute;
    left: 15.5em;
    top: 1.3em;
    font-size: 13px;
    font-style: normal;
    font-weight: 400;
    letter-spacing: 0em;
    color: $text-color;
  }
  h4 {
    position: absolute;
    left: 11.8em;
    top: 2.5em;
    padding-right: 0.3em;
    font-size: 17px;
    font-style: normal;
    font-weight: 600;
    letter-spacing: 0em;
  }
  .original-price {
    position: absolute;
    font-size: 13px;
    font-style: normal;
    font-weight: 700;
    color: $price-color;
    line-height: 15px;
    letter-spacing: 0em;
    left: 16.15em;
    top: 9.2em;
    text-decoration: line-through;
  }
  .discount-box {
    position: absolute;
    border-radius: 3px;
    background-color: $discount-color;
    left: 20.25em;
    top: 7.7em;
  }
  .discount-price {
    position: absolute;
    padding: 0.3em 0.4em;
    font-size: 15px;
    font-style: normal;
    font-weight: 700;
    line-height: 18px;
    letter-spacing: 0em;
    color: $discount-text-color;
  }

  .original-price-only {
    position: absolute;
    padding: 0.2em 0.3em;
    left: 13.3em;
    top: 7.5em;
    font-size: 15px;
    font-style: normal;
    font-weight: 700;
    letter-spacing: 0em;
    color: $discount-text-color;
    cursor: pointer;
  }

  .original-price-box {
    position: absolute;
    border-radius: 3px;
    background-color: $discount-color;
    cursor: pointer;
  }
}

@media screen and (max-width: 425px) {
  #selection-bar {
    grid-template-columns: repeat(auto-fit, minmax(300px, 453px));
    justify-content: center;
    padding: 2em 1em;
    gap: 1em;
    .selected-card {
      p {
        font-size: 10px;
        left: 19em;
      }
      h4 {
        font-size: 13px;
        left: 14.6em;
        padding-right: 0.7em;
      }
      .original-price {
        font-size: 11px;
        top: 8.2em;
        left: 17.4em;
      }
      .discount-price {
        font-size: 13px;
        left: 14.7em;
        top: 9.2em;
      }
      .original-price-only {
        font-size: 13px;
        left: 14.7em;
      }
    }
  }
}
</style>
