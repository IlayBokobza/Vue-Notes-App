<template>
  <div id="app">
    <navbar class="navbar" />
    <router-view class="site" />
  </div>
</template>
<script>
import navbar from "./components/Navbar";
export default {
  data() {
    return {};
  },
  components: {
    navbar,
  },
};
</script>
<style>
body {
  padding: 0;
  margin: 0;
}
p {
  font-family: Arial, Helvetica, sans-serif;
}
h1 {
  font-family: Arial, Helvetica, sans-serif;
}
h2 {
  font-weight: 400;
  font-family: Arial, Helvetica, sans-serif;
}
a {
  text-decoration: none;
}
a:hover {
  text-decoration: none;
}
a h3 {
  font-size: 18px;
}
.fas {
  color: gray;
}
.note {
  width: 240px;
  margin: 20px;
  max-height: 378px;
  height: fit-content;
  overflow: hidden;
  hyphens: auto;
  border: 1px solid;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.note h2 {
  margin: 0;
  margin-top: 10px;
  font-weight: 500;
}
.note h3 {
  padding: 0 20px;
  font-weight: 300;
}
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.divider{
  width: 100%;
  height: 1px;
  border-bottom: solid 1px;
  margin: 50px auto;
}
/* note container */
.notes {
  width: 70%;
  display: flex;
  flex-wrap: wrap;
}
#cover {
  opacity: 0.8;
  position: fixed;
  background: gray;
  width: 100%;
  height: 100%;
  z-index: 1;
  left: 0px;
  top: 0;
}
</style>
