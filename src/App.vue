<template>
  <div id="wrapper">
    <navbar :appTitle="appTitle" />
    <categories :search="search" @selected="onSelected" :filteredList="filteredList" @query-change="setSearchValue" />
    <section class="cards">
      <div class="card" v-for="(product, index) in filteredList" :key="index" @click="setActive(index)" :class="{ active: activeIndex === index }">
        <img :src="'/images/' + product.image" :alt="product.image" @click="isSelected = true" />
        <span class="brand" @click="isSelected = true">{{ product.brand }} </span>
        <span class="title" @click="isSelected = true">{{ product.title }}</span>
        <span v-if="product.price.finalPrice < product.price.regularPrice">
          <span class="original-price" @click="showSelected">{{ product.price.regularPrice }} {{ product.price.currency }}</span>
          <span class="discount-price discount-box" @click="isSelected = true">{{ product.price.finalPrice }} {{ product.price.currency }}</span>
        </span>
        <span class="original-price-only original-price-box" v-else @click="isSelected = true">{{ product.price.regularPrice }} {{ product.price.currency }}</span>
      </div>
    </section>
    <display-selected :products="products" :isSelected="isSelected" :filteredList="filteredList" :activeIndex="activeIndex" />
  </div>
</template>

<script>
import navbar from "./components/navbar.vue";
import categories from "./components/categories.vue";
import displaySelected from "./components/displaySelected.vue";

import { appTitle } from "./data/data";
import { products } from "./data/data";

export default {
  name: "App",
  components: {
    navbar,
    categories,
    displaySelected,
  },
  data() {
    return {
      appTitle,
      products,
      activeIndex: 0,
      search: "",
      sort: "",
      isSelected: false,
    };
  },
  methods: {
    setActive(index) {
      this.activeIndex = index;
    },
    showSelected() {
      this.isSelected = true;
    },
    setSearchValue(value) {
      this.search = value;
    },
    onSelected(value) {
      this.sort = value;
    },
  },
  computed: {
    filteredList() {
      let products = this.products.filter((product) => {
        return product.brand.toLowerCase().includes(this.search.toLowerCase()) || product.title.toLowerCase().includes(this.search.toLowerCase()) || product.price.finalPrice.toString().includes(this.search.toString()) || product.price.regularPrice.toString().includes(this.search.toString());
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
    },
  },
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
$font: "Roboto", sans-serif;
* {
  margin: 0;
  padding: 0;
  border: 0;
  box-sizing: border-box;
  font-family: $font;
}
html {
  background-color: $discount-text-color;
}

#wrapper {
  overflow-x: hidden; // this prevent horizontal scroll
  height: 100vh;
  margin: 0 auto;
  max-width: 1440px;
}

.cards {
  display: grid;
  padding: 2em 1em;
  grid-template-columns: repeat(auto-fit, minmax(453px, 1fr));
  gap: 1em;

  .card {
    position: relative;
    display: block;
    height: 170px;
    width: 100%;
    border-radius: 5px;
    border: 1px solid #d0d0d0;
    background-color: $background-color2;
    &:hover {
      border: 1px solid $discount-color;
      margin: -1px;
      transform: scale(1.02);
    }
  }

  .active {
    border: 1px solid $discount-color;
    margin: -1px;
  }
  img {
    position: absolute;
    max-width: 179px;
    height: 100%;
    left: 0;
    top: 0;
    border-radius: 5px 0px 0px 5px;
    cursor: pointer;
  }
  .brand {
    display: block;
    position: relative;
    left: 15em;
    top: 1.3em;
    font-size: 13px;
    font-style: normal;
    font-weight: 400;
    line-height: 1em;
    letter-spacing: 0em;
    color: $text-color;
    cursor: pointer;
  }
  .title {
    position: absolute;
    display: block;
    font-weight: 600;
    top: 2.5em;
    left: 12em;
    padding-right: 0.4em;
    cursor: pointer;
  }
  .original-price {
    position: absolute;
    height: 1em;
    font-size: 13px;
    font-style: normal;
    font-weight: 700;
    color: $price-color;
    line-height: 1em;
    letter-spacing: 0em;
    left: 15em;
    top: 8.8em;
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
    top: 7.2em;
    left: 19em;
    padding: 0.3em 0.4em;
    font-size: 15px;
    font-style: normal;
    font-weight: 700;
    line-height: 1.1em;
    letter-spacing: 0em;
    color: $discount-text-color;
    cursor: pointer;
  }

  .original-price-only {
    position: absolute;
    width: auto;
    padding: 0.3em 0.4em;
    left: 13em;
    top: 7.15em;
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
    cursor: pointer;
  }
}

/* Media Queries */

@media screen and (max-width: 1422px) and (min-width: 1024.01px) {
  .top-container {
    justify-content: center;
    #search {
      input {
        width: 400px;
      }
    }
    #sortby {
      select {
        width: 400px;
      }
    }
  }
  .cards {
    grid-template-columns: repeat(auto-fit, minmax(400px, 35%));
    justify-content: center;
  }
}

@media screen and (max-width: 1024px) and (min-width: 860px) {
  .top-container {
    justify-content: center;
    #search {
      input {
        width: 400px;
      }
    }
    #sortby {
      select {
        width: 400px;
      }
    }
  }
  .cards {
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
    justify-content: center;
  }
}

@media screen and (max-width: 860px) and (min-width: 688.01px) {
  .cards {
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    justify-content: center;

    .original-price {
      font-size: 11px;
      top: 10em;
      left: 17.4em;
    }
    .discount-price {
      font-size: 13px;
      left: 14.7em;
      top: 10.2em;
    }
    .original-price-only {
      font-size: 13px;
      left: 14.7em;
      top: 9em;
    }
  }

  .top-container {
    justify-content: center;
    #search {
      input {
        width: 300px;
      }
    }
    #sortby {
      select {
        width: 300px;
      }
    }
  }
}

@media screen and (max-width: 688px) {
  .top-container {
    grid-template-columns: 1fr;
    grid-template-areas:
      "category"
      "search"
      "sortby";
    #search {
      input {
        width: 412.4px;
      }
    }
    #sortby {
      select {
        width: 412.4px;
      }
    }
  }

  .cards {
    grid-template-columns: repeat(auto-fit, minmax(300px, 453px));
    justify-content: center;
  }
}

@media screen and (max-width: 425px) {
  .top-container {
    #search {
      input {
        width: 320px;
      }
    }
    #sortby {
      select {
        width: 320px;
      }
    }
  }
  .cards {
    .brand {
      font-size: 10px;
      left: 19em;
    }
    .title {
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
</style>
