<template>
  <div id="app">
    <Sidebar></Sidebar>
    <router-view></router-view>
  </div>
</template>

<script>
import Sidebar from "@/components/Sidebar.vue";
export default {
  name: "app",
  components: {
    Sidebar
  }
};
</script>

<style>
/*@import '//at.alicdn.com/t/font_496303_kqrjhri8l25d0a4i.css';*/
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
html,
body,
#app {
  height: 100%;
}
body {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  font-size: 14px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: rgb(245, 244, 244);
}
a {
  text-decoration: none;
  color: #444;
}
ul,
li {
  list-style: none;
}
.btn {
  color: #666;
  font-size: 12px;
  padding: 0px 4px;
  background-color: #fff;
  box-shadow: 0px 0px 2px 0px #ccc;
  border: none;
  cursor: pointer;
  display: inline-block;
}
#app {
  display: flex;
  align-items: stretch;
}
</style>
