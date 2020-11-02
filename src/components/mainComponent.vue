<template>
  <div class="wrapper">
    <nav class="main-nav">
      <ul>
        <li>
          <a href="#">{{ appTitle }}</a>
        </li>
      </ul>
    </nav>

    <section class="top-container">
      <h3 class="category">Categories</h3>
      <div class="search">
        <input type="text" placeholder="Search" v-model="search" />
      </div>
      <div class="sortby">
        <select v-model="sort">
          <option value="" selected>Sort By</option>
          <option
            v-for="option in options"
            :key="option.label"
            :label="option.label"
            :value="option.value"
          ></option>
        </select>
      </div>
    </section>

    <section class="cards">
      <div
        class="card"
        v-for="(product, index) in filteredList"
        :key="index"
        @click="setActive(index)"
        :class="{ active: activeIndex === index }"
      >
        <img :src="'/images/' + product.image" :alt="product.image" />
        <span class="brand">{{ product.brand }}</span>
        <span class="title">{{ product.title }}</span>
        <span v-if="product.price.finalPrice < product.price.regularPrice">
          <span class="original-price"
            >{{ product.price.regularPrice }} {{ product.price.currency }}</span
          >
          <span class="discount-price discount-box"
            >{{ product.price.finalPrice }} {{ product.price.currency }}</span
          >
        </span>
        <span class="original-price-only original-price-box" v-else
          >{{ product.price.regularPrice }} {{ product.price.currency }}</span
        >
      </div>
    </section>
  </div>
</template>

<script>
import { appTitle } from "../data/data";
import { products } from "../data/data";
import { options } from "../data/data";

export default {
  name: "mainComponent",
  data() {
    return {
      appTitle,
      products,
      options,
      activeIndex: undefined,
      search: "",
      sort: "",
      isSelected: true
    };
  },
  methods: {
    setActive(index) {
      this.activeIndex = index;
    }
  },
  computed: {
    filteredList() {
      let products = this.products.filter(product => {
        return (
          product.brand.toLowerCase().includes(this.search.toLowerCase()) ||
          product.title.toLowerCase().includes(this.search.toLowerCase()) ||
          product.price.finalPrice
            .toString()
            .includes(this.search.toString()) ||
          product.price.regularPrice.toString().includes(this.search.toString())
        );
      });

      if (this.sort == "brand") {
        return products.slice().sort((a, b) => (a.brand > b.brand ? 1 : -1));
      } else if (this.sort == "title") {
        return products.slice().sort((a, b) => (a.title > b.title ? 1 : -1));
      }
      if (this.sort == "finalPrice") {
        return products.sort((a, b) => b.price.finalPrice - a.price.finalPrice);
      } else {
        return products;
      }
    }
  }
};
</script>
<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap");
$background-color1: #e5e5e5;
$border-color: #d0d0d0;
$background-color2: #fafafa;
$price-color: #777777;
$discount-color: #ff0000;
$discount-text-color: #ffffff;
$text-color: #000000;
$selected-background: #e7e7e7;
$selected-color: #000000;
$font: "Roboto", sans-serif;
* {
  margin: 0;
  padding: 0;
}
html {
  background-color: $background-color1;
}
body {
  font-family: $font;
}

img {
  max-width: 100%;
}

.wrapper {
  z-index: -1;
  display: grid;
  height: 100vh;
}

.main-nav {
  background-color: $discount-text-color;
  position: fixed;
  width: 100%;
  z-index: 1;
  margin-bottom: 20px;
  ul {
    padding: 0;
    list-style: none;
    box-shadow: 0px 4px 25px rgba(0, 0, 0, 0.15);
    min-height: 75px;
    min-width: 95px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
  }
  a {
    margin-top: 26px;
    margin-bottom: 26px;
    text-transform: uppercase;
    line-height: 23px;
    text-align: left;
    color: $text-color;
    cursor: pointer;
    place-items: start;
    margin-left: 5rem;
    grid-auto-flow: column;
    align-content: space-evenly;
    font-style: normal;
    font-weight: 400;
    display: grid;
    text-decoration: none;
    font-size: 20px;
    letter-spacing: 0em;
  }
}

.top-container {
  margin: 0 239px 0 240px;
  display: grid;
  grid-template-areas:
    "category category "
    "search  sortby";
  justify-items: space-between;
  h3 {
    grid-area: category;
    width: 102px;
    height: 29px;
    margin-top: 105px;
    margin-bottom: 30px;
    font-family: $font;
    font-style: normal;
    font-weight: 500;
    font-size: 25px;
    line-height: 29px;
    color: $text-color;
    cursor: default;
  }
  input {
    grid-area: search;
    width: 453px;
    height: 48px;
    background: $discount-text-color;
    border: 1px solid $price-color;
    color: $price-color;
    border-radius: 5px;
    padding-left: 52px;
    background-image: url(../../public/images/icons/search-icon.svg);
    background-repeat: no-repeat;
    background-position: 19px center;
    box-sizing: border-box;
    margin-bottom: 30px;
  }

  select {
    margin-left: auto;
    grid-area: sortby;
    width: 330px;
    height: 48px;
    padding-left: 16px;
    padding-top: 15px;
    padding-bottom: 15px;
    background: $discount-text-color;
    border: 1px solid $price-color;
    color: $price-color;
    border-radius: 5px;
    display: grid;
    box-sizing: border-box;
  }
}

.cards {
  margin: 0 239px 0 240px;
  display: grid;
  grid-gap: 41px;
  column-gap: 40px;
  grid-template-columns: repeat(auto-fill, minmax(410px, 3fr));
  .card {
    position: relative;
    text-align: center;
    display: block;
    height: 170px;
    max-width: 453px;
    left: 0px;
    top: 0px;
    border-radius: 5px;
    border: 1px solid #d0d0d0;
    text-align: left;
  }
  img {
    position: absolute;
    width: 179px;
    height: 100%;
    left: 0%;
    top: 0%;
    border-radius: 5px 0px 0px 5px;
  }
  .brand {
    display: block;
    margin-left: 203px;
    margin-top: 42px;
    font-size: 13px;
    font-style: normal;
    font-weight: 400;
    line-height: 15px;
    letter-spacing: 0em;
    color: $text-color;
  }
  .title {
    display: block;
    margin-top: 8px;
    margin-left: 203px;
    padding: 0;
  }
  .original-price {
    position: absolute;
    height: 15px;
    width: 70px;
    font-family: $font;
    font-size: 13px;
    font-style: normal;
    font-weight: 700;
    color: $price-color;
    line-height: 15px;
    letter-spacing: 0em;
    left: 44.81%;
    top: 68%;
    text-decoration: line-through;
  }

  .discount-box {
    border-radius: 3px;
    background-color: $discount-color;
  }

  .discount-price {
    position: absolute;
    min-width: 60px;
    top: 70%;
    left: 80%;
    margin: -10px 0px 0px -36px;
    padding: 4px;
    font-family: $font;
    font-size: 15px;
    font-style: normal;
    font-weight: 700;
    line-height: 18px;
    letter-spacing: 0em;
    color: $discount-text-color;
  }

  .original-price-only {
    position: absolute;
    min-height: 18px;
    width: 80px;
    left: 55%;
    top: 79%;
    margin: -10px 0px 0px -36px;
    font-family: $font;
    font-size: 15px;
    font-style: normal;
    font-weight: 700;
    letter-spacing: 0em;
    color: $discount-text-color;
  }

  .original-price-box {
    position: absolute;
    min-width: 68px;
    border-radius: 3px;
    background-color: $discount-color;
    left: 50%;
    top: 70%;
    padding: 4px;
  }
}

/* Media Queries */
@media (max-width: 700px) {
  .top-container {
    grid-template-areas:
      "category category "
      "search search"
      "sortby sortby";
  }

  .main-nav ul {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 500px) {
  .top-container {
    grid-template-areas:
      "category"
      "search"
      "sortby";
  }
}

@media (max-width: 1300px) {
  .top-container {
    grid-template-areas:
      "category"
      "search"
      "sortby";
    justify-content: center;
    h3 {
    justify-content: center;
    }
  }
  .sortby {
    display: grid;
    justify-content: center;
    .select {
      margin: 0 0 30px 0;
    }
  }
  .top-container {
    margin-bottom: 30px;
  }
  .cards .original-price-only {
    margin-left: 50px;
    margin-top: 14px;
  }
  .cards .discount-price {
    margin-top: 14px;
  }
  .cards .original-price{
    margin-top: -5px;
    margin-left: 95px;
  }
}

@media (max-width: 1800px) {
  .cards {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 3fr));
  }
}
</style>
