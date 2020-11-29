<template>
  <nav class="main-nav">
    <a href="#">{{ appTitle }}</a>
  </nav>
</template>

<script>
export default {
  props: ["appTitle"],
};
</script>

<style lang="scss">
$discount-text-color: #ffffff;
$text-color: #000000;

.main-nav {
  background-color: $discount-text-color;
  z-index: 1;
  position: fixed;
  margin: 0;
  padding: 0;
  width: 1440px;
  box-shadow: 0px 4px 25px rgba(0, 0, 0, 0.15);
  a {
    display: block;
    padding: 1.2em 2em;
    text-transform: uppercase;
    line-height: 23px;
    color: $text-color;
    cursor: pointer;
    font-style: normal;
    font-weight: 400;
    text-decoration: none;
    font-size: 20px;
    letter-spacing: 0em;
  }
}
</style>
