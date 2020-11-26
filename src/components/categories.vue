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
