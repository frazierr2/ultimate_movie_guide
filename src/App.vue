<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style>
body,
html {
  margin: 0;
  padding: 0;
}
* {
  box-sizing: border-box;
}
#app {
  background: linear-gradient(
      rgba(110, 110, 110, 0.6),
      rgba(110, 110, 110, 0.6)
    ),
    url("https://cdn.pixabay.com/photo/2018/10/26/11/14/tv-3774381_960_720.jpg");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: black;
  height: 100vh;
  position: relative;
}
#nav {
  padding: 30px;
  text-align: left;
  padding-bottom: 0;
}

#nav a {
  font-weight: 100;
  color: white;
}

#nav a.router-link-exact-active {
  color: red;
}
</style>
