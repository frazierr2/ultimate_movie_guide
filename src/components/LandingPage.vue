<template>
  <div class="hello">
    <h1 id="app-name">Ultimate Movie Guide</h1>
    <div>
      <h2 id="caption-one">See what's next.</h2>
      <h4 id="caption-two">WATCH ANYWHERE, ANYTIME</h4>

      <router-link to="/search" class="search-button-link">
        <div id="search-button">
          SEARCH MOVIES
          <span id="search-caret">
            <font-awesome-icon icon="angle-right"/>
          </span>
        </div>
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: "LandingPage",
  props: {},
  data() {
    return {};
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  top: 25%;
  position: absolute;
  left: 25%;
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
.search-button-link {
  text-decoration: none;
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

/* Responsive Media Queries */
@media only screen and (min-width: 768px) and (max-width: 991px) {
  #app-name {
    font-size: 73px;
  }
  #caption-one {
    font-size: 50px;
  }
  #caption-two {
    font-size: 25px;
  }
  .hello {
    position: absolute;
    top: 50%;
    left: 30%;
    transform: translate(-18%, -50%);
  }
}
@media only screen and (max-width: 767px) {
  #app-name {
    font-size: 54px;
  }
  #caption-one {
    font-size: 40px;
  }
  #caption-two {
    font-size: 20px;
  }
  .hello {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}
</style>
