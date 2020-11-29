<template>
  <div class="top-container">
    <div id="category">
      <h3>Categories</h3>
    </div>
    <div id="search">
      <input
        type="text"
        placeholder="Search"
        v-model="searchQuery"
        @input="onInput"
      />
    </div>
    <div id="sortby">
      <select v-model="sortBy" @change="changeOption">
        <option value="" selected>Sort By</option>
        <option
          v-for="(option, id) in options"
          :key="id"
          :label="option.label"
          :value="option.value"
        ></option>
      </select>
    </div>
  </div>
</template>

<script>
import { options } from "../data/data";

export default {
  name: "categories",
  data() {
    return {
      options,
      searchQuery: "",
      sortBy: "",
    };
  },
  props: ["sort"],
  methods: {
    onInput() {
      this.$emit("query-change", this.searchQuery);
    },
    changeOption() {
      this.$emit("selected", this.sortBy);
    },
  },
};
</script>

<style lang="scss">
$price-color: #777777;
$discount-text-color: #ffffff;
$text-color: #000000;

.top-container {
  margin-top: 7em;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  grid-template-areas:
    "category category"
    "search sortby";
  #category {
    h3 {
      grid-area: category;
      font-style: normal;
      font-weight: 500;
      font-size: 25px;
      line-height: 29px;
      color: $text-color;
      cursor: default;
      margin-left: 5.2em;
    }
  }
  #search {
    grid-area: search;
    padding-top: 1em;
    margin: 0 auto;
    input {
      width: 453px;
      height: 48px;
      background: $discount-text-color;
      border: 1px solid $price-color;
      color: $price-color;
      border-radius: 5px;
      padding-left: 4em;
      background-image: url(../../public/images/icons/search-icon.svg);
      background-repeat: no-repeat;
      background-position: 1.8em center;
      box-sizing: border-box;
    }
  }

  #sortby {
    grid-area: sortby;
    position: relative;
    padding-top: 1em;
    margin: 0 auto;
    select {
      width: 330px;
      height: 48px;
      padding-left: 1em;
      padding-top: 1.1em;
      padding-bottom: 1.1em;
      background: $discount-text-color;
      border: 1px solid $price-color;
      color: $price-color;
      border-radius: 5px;
      box-sizing: border-box;
    }
  }
}
  
</style>