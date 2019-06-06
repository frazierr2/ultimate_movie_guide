<template>
  <div class="hello">
    <!-- <b-jumbotron header="BootstrapVue" lead="Bootstrap 4 Components for Vue.js 2">
      <p>For more information visit website</p>
      <b-button variant="primary" href="#">More Info</b-button>
    </b-jumbotron>-->
    <h1 id="app-name">Ultimate Movie Guide</h1>
    <div>
      <h2 id="caption-one">See what's next.</h2>
      <h4 id="caption-two">WATCH ANYWHERE, ANYTIME</h4>

      <div id="search-button">
        SEARCH MOVIES
        <span id="search-caret">
          <font-awesome-icon icon="angle-right"/>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "LandingPage",
  props: {
    msg: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  /* padding: 0 15px; */
}
#app-name {
  font-family: "Bangers", cursive;
  font-size: 100px;
  color: #e50614;
}
#caption-one {
  font-size: 70px;
  color: white;
}
#caption-two {
  font-size: 40px;
  color: white;
}
#search-button {
  background-color: #e50614;
  color: white;
  width: 400px;
  font-size: 30px;
  padding: 30px;
  font-family: "Poppins", sans-serif;
  margin: 60px auto 0;
}
#search-caret {
  padding-left: 20px;
  transform: rotate(45deg);
}
</style>
