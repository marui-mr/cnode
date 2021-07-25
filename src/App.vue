<template>
  <div id="app">
    <Header></Header>
    <div class="main">
      <router-view name="main"></router-view>
      <router-view name="slideBar"></router-view>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header";
export default {
  name: "App",
  components: {
    Header,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
a {
  text-decoration: none;
  text-shadow: none;
  cursor: pointer;
}
img {
  max-width: 100%;
  vertical-align: middle;
}
body {
  background-color: #e1e1e1;
}
.main {
  width: 80%;
  margin: 0 auto;
  display: flex;
}
</style>
