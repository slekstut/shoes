<template>
  <div id="wrapper">
    <nav class="main-nav">
          <a href="#">{{ appTitle }}</a>
    </nav>

    <div class="top-container">
      <div id="category">
        <h3>Categories</h3>
      </div>
      <div id="search">
        <input type="text" placeholder="Search" v-model="search" />
      </div>
      <div id="sortby">
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
    </div>

    <section class="cards">
      <div
        class="card"
        v-for="(product, index) in filteredList"
        :key="index"
        @click="setActive(index)"
        :class="{ active: activeIndex === index }"
      >
        <img
          :src="'/images/' + product.image"
          :alt="product.image"
          @click="isSelected = true"
        />
        <span class="brand" @click="isSelected = true"
          >{{ product.brand }}
        </span>
        <span class="title" @click="isSelected = true">{{
          product.title
        }}</span>
        <span v-if="product.price.finalPrice < product.price.regularPrice">
          <span class="original-price" @click="showSelected"
            >{{ product.price.regularPrice }} {{ product.price.currency }}</span
          >
          <span class="discount-price discount-box" @click="isSelected = true"
            >{{ product.price.finalPrice }} {{ product.price.currency }}</span
          >
        </span>
        <span
          class="original-price-only original-price-box"
          v-else
          @click="isSelected = true"
          >{{ product.price.regularPrice }} {{ product.price.currency }}</span
        >
      </div>
    </section>
    <div id="selection-bar">
      <h2>Selected product</h2>
      <div class="selected-card" v-if="isSelected">
        <img :src="'/images/' + activeCard.image" alt="product-img" />
        <p>{{ activeCard.brand }}</p>
        <h4>{{ activeCard.title }}</h4>
        <span
          v-if="activeCard.price.finalPrice < activeCard.price.regularPrice"
        >
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
      activeIndex: 0,
      search: "",
      sort: "",
      isSelected: false
    };
  },
  methods: {
    setActive(index) {
      this.activeIndex = index;
    },
    showSelected() {
      this.isSelected = true;
    }
  },
  computed: {
    activeCard() {
      return this.products[this.activeIndex];
    },
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
  box-sizing: border-box;
}
html {
  background-color: $discount-text-color;
  width: 100%;
  margin: 0 auto;
  
}
body {
  font-family: $font;
}

#wrapper {
  position: relative;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  min-width: 240px;
  max-width: 1024px;
  margin: 0 auto;
}

.main-nav {
  grid-area: navbar;
  background-color: $discount-text-color;
  z-index: 1;
  position: fixed;
  top: 0;
  width: 100%;
  box-shadow: 0px 4px 25px rgba(0, 0, 0, 0.15);
  a {
    width: 100%;
    display: block;
    float: left;
    padding: 20px 26px;
    text-transform: uppercase;
    line-height: 23px;
    color: $text-color;
    cursor: pointer;
    align-content: space-evenly;
    font-style: normal;
    font-weight: 400;
    text-decoration: none;
    font-size: 20px;
    letter-spacing: 0em;
  }
}

.top-container {
  display: grid;
  z-index: -1;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  grid-template-areas:
    "category category category category"
    "search search sortby sortby";
    #category{
      h3{
          grid-area: category;
          width: 102px;
          height: 29px;
          font-family: $font;
          font-style: normal;
          font-weight: 500;
          font-size: 25px;
          line-height: 29px;
          color: $text-color;
          cursor: default;
      }
          
  }
    #search {
        grid-area: search;
      input {
        width: 453px;
        height: 48px;
        margin-bottom: 30px;
        background: $discount-text-color;
        border: 1px solid $price-color;
        color: $price-color;
        border-radius: 5px;
        padding-left: 52px;
        background-image: url(../../public/images/icons/search-icon.svg);
        background-repeat: no-repeat;
        background-position: 19px center;
        box-sizing: border-box;
      }
    
  }

  #sortby {
    grid-area: sortby;
    position: relative;
    select {
      margin-bottom: 30px;
      width: 330px;
      height: 48px;
      padding-left: 16px;
      padding-top: 15px;
      padding-bottom: 15px;
      background: $discount-text-color;
      border: 1px solid $price-color;
      color: $price-color;
      border-radius: 5px;
      box-sizing: border-box;
    }
  }
}

.cards {
  display: grid;
  grid-gap: 41px;
  grid-template-columns: repeat(auto-fit, minmax(430px, 1fr));
  
  .card {
    position: relative;
    display: block;
    height: 170px;
    width: 100%;
    border-radius: 5px;
    border: 1px solid #d0d0d0;
    background-color: $background-color2;
    &:hover {
      border: 3px solid $discount-color;
      margin: -3px;
    }
    
  }
  
  .active {
    border: 3px solid $discount-color;
    margin: -3px;
  }
  img {
    position: absolute;
    max-width: 179px;
    height: 100%;
    left: 0%;
    top: 0%;
    border-radius: 5px 0px 0px 5px;
    cursor: pointer;
  }
  .brand {
    display: block;
    position: relative;
    left: 200px;
    top: 20px;
    font-size: 13px;
    font-style: normal;
    font-weight: 400;
    line-height: 15px;
    letter-spacing: 0em;
    color: $text-color;
    cursor: pointer;
  }
  .title {
    position: absolute;
    top: 40px;
    left: 200px;
    padding: 0;
    cursor: pointer;
  }
  .original-price {
    position: absolute;
    height: 15px;
    width: 100%;
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
    cursor: pointer;
  }

  .discount-box {
    border-radius: 3px;
    background-color: $discount-color;
    cursor: pointer;
  }

  .discount-price {
    position: absolute;
    width: auto;
    top: 70%;
    left: 70%;
    margin: -10px 0px 0px -36px;
    padding: 4px;
    font-family: $font;
    font-size: 15px;
    font-style: normal;
    font-weight: 700;
    line-height: 18px;
    letter-spacing: 0em;
    color: $discount-text-color;
    cursor: pointer;
  }

  .original-price-only {
    position: absolute;
    min-height: 18px;
    width: auto;
    left: 55%;
    top: 79%;
    margin: -10px 0px 0px -36px;
    font-family: $font;
    font-size: 15px;
    font-style: normal;
    font-weight: 700;
    letter-spacing: 0em;
    color: $discount-text-color;
    cursor: pointer;
  }

  .original-price-box {
    position: absolute;
    width: auto;
    border-radius: 3px;
    background-color: $discount-color;
    margin-left: -50px;
    top: 70%;
    padding: 4px;
    cursor: pointer;
  }
}

#selection-bar {
  display: grid;
  grid-gap: 9px;
  grid-template-rows: repeat(2, 100px);
  background: $selected-background;
  min-height: 319px;
  margin-bottom: 30px;
  h2 {
    height: 29px;
    width: auto;
    margin-top: 40px;
    font-style: normal;
    font-weight: 500;
    font-size: 25px;
    line-height: 29px;
    justify-self: center;
  }
  .selected-card {
    position: relative;
    height: 170px;
    width: 100%;
    border-radius: 5px;
    background-color: $background-color2;
    border: 1px solid $border-color;
    border-radius: 5px;
    justify-self: center;
  }
  img {
    position: relative;
    height: 100%;
    width: 179px;
    left: 0%;
    top: 0%;
    bottom: 0%;
    border-radius: 5px 0px 0px 5px;
  }
  p {
    position: absolute;
    height: 15px;
    width: 135px;
    left: 203px;
    top: 42px;
    font-size: 13px;
    font-style: normal;
    font-weight: 400;
    line-height: 15px;
    letter-spacing: 0em;
    color: $text-color;
  }
  h4 {
    position: absolute;
    height: 20px;
    width: 182px;
    left: 44.81%;
    top: 38.24%;
    font-family: $font;
    font-size: 17px;
    font-style: normal;
    font-weight: 500;
    line-height: 20px;
    letter-spacing: 0em;
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
    top: 75%;
    text-decoration: line-through;
  }
  .discount-box {
    position: absolute;
    min-width: 68px;
    border-radius: 3px;
    background-color: $discount-color;
    left: 59.82%;
    top: 70%;
  }
  .discount-price {
    position: absolute;
    height: 23px;
    width: 80px;
    padding-top: 4px;
    margin-left: 20px;
    font-family: $font;
    font-size: 15px;
    font-style: normal;
    font-weight: 700;
    line-height: 18px;
    letter-spacing: 0em;
    color: $discount-text-color;
    text-align: center;
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
    cursor: pointer;
  }

  .original-price-box {
    position: absolute;
    min-width: 68px;
    border-radius: 3px;
    background-color: $discount-color;
    margin-left: -20px;
    top: 70%;
    padding: 4px;
    cursor: pointer;
  }
}

/* Media Queries */

@media (max-width: 500px) {
  body {
    font-size:12px;
  }

  .main-nav {
    height: 2rem;
    a {
    padding: 5px 20px;
    font-size: 10px;
    }
  }
  .cards {
    margin: 0 0 10px 0;
    grid-template-columns: 1fr;
    gap: 2rem;
    align-items: center;
    justify-items: center;
    .card {
      .brand {
        top: 11px;
      }
      width: 100%;
      .original-price {
        left: 12.5rem;
        top: 6rem;
      }
      .original-price-only {
        margin-left: 30px;
        width: auto;
      }
      .discount-price {
        left: 14.5rem;
        top: 8.5rem;
      }
    }
  }
  .top-container {
    margin: 0;
    padding-top: 40px;
    grid-template-columns: 1fr;
    justify-items: center;
    align-items: center;
    input {
      max-width: 300px;
    }
    select {
      max-width: 300px;
    }
  }
  #selection-bar {
    margin: 0;
    margin-bottom: 30px;
    .original-price-only {
      margin-left: 25px;
      margin-top: 5px;
      width: auto;
    }
    p {
      margin-top: -22px;
    }
    h4 {
      margin-left: 60px;
      margin-top: -20px;
      width: auto;
    }
     .original-price {
        margin-left: 60px;
        margin-top: -20px;
      }
       .discount-price {
        margin-left: 5px;
        margin-top: 10px;
      }
  }
}
// Large devices (desktops, 992px and up)
@media (max-width: 992px) {
  .main-nav {
    grid-template-areas:
    "navbar";
  }
  .top-container {
    grid-template-areas:
      "category"
      "search"
      "sortby";
  }
}
// Extra large devices (desktops, 1440px and down)
@media (min-width: 1440px) {
  }
</style>
