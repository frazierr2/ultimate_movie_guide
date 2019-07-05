<template>
  <b-col class="list-column" cols="10" md="11">
    <b-row>
      <b-col>
        <p class="selected-option-text" col>{{ selectedOption }}</p>
      </b-col>

      <b-col class="sort-button">
        <span v-if="orderStatus !== 'Sort'">
          <font-awesome-icon
            class="sort-icon"
            v-if="!orderAscDescOrientation"
            icon="long-arrow-alt-up"
          />
          <font-awesome-icon
            class="sort-icon"
            v-if="orderAscDescOrientation"
            icon="long-arrow-alt-down"
          />
        </span>

        <span
          @click="sortArrays(orderAscDescOrientation = !orderAscDescOrientation)"
          class="button-for-sort"
        >{{ orderStatus }}</span>
      </b-col>
    </b-row>

    <b-row v-if="loading">
      <div id="loading-wrapper">
        <div id="loading-text">LOADING...</div>
        <div id="loading-content"></div>
      </div>
    </b-row>

    <b-row v-if="moviesReturned" name="fade" is="transition-group">
      <b-col v-for="(movie, index) in moviesArray[0]" :key="index" cols="12" sm="6" md="3">
        <img
          class="movie-banner-picture"
          :src="'//image.tmdb.org/t/p/h632/'+ movie.poster_path"
          @click="getDetails(index)"
        />
        <p class="text-muted">{{ movie.original_title }}</p>
      </b-col>
    </b-row>
  </b-col>
</template>


<script>
import _ from "lodash";
export default {
  props: [
    "selectedOption",
    "orderStatus",
    "orderAscDesc",
    "moviesReturned",
    "moviesArray",
    "loading"
  ],
  data() {
    return {
      orderStatusText: "",
      orderAscDescOrientation: this.orderAscDesc,
      oldMoviesArray: this.moviesArray,
      newMoviesArray: []
    };
  },
  watch: {
    orderStatus(newValue) {
      this.orderStatusText = newValue;
    }
  },
  methods: {
    sortArrays(arrayOrder) {
      this.newMoviesArray = [];
      var orderOfArray;
      if (arrayOrder == true) {
        this.orderStatusText = "A-Z";
        orderOfArray = _.orderBy(this.moviesArray[0], "title", "asc");
      } else {
        orderOfArray = _.orderBy(this.moviesArray[0], "title", "desc");
        this.orderStatusText = "Z-A";
      }
      this.newMoviesArray.push(orderOfArray);

      this.$emit("newMoviesArray", this.newMoviesArray, this.orderStatusText);
    }
  },
  computed: {}
};
</script>



<style scoped>
.selected-option-text {
  text-align: left;
  margin: 10px 0 10px 50px;
}
.list-column {
  background-color: rgba(255, 255, 255, 0.7);
}
.sort-icon {
  color: #cd6155;
}
.sort-button {
  text-align: right;
  top: 10px;
  right: 20px;
}
.button-for-sort {
  margin-left: 10px;
  border: 1px solid #808b96;
  background-color: #808b96;
  border-radius: 4px;
  padding: 4px 10px;
  color: white;
}
.movie-banner-picture {
  width: 90%;
  height: 300px;
  margin: 5px 0;
  padding: 5px;
  box-shadow: 4px 4px 10px #566573;
}
/* ==================================================== */
/* ================== Loading Styles ================== */
/* ==================================================== */
#loading-wrapper {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
}

#loading-text {
  display: block;
  position: absolute;
  top: 50%;
  left: 50%;
  color: #999;
  width: 100px;
  height: 30px;
  margin: -7px 0 0 -45px;
  text-align: center;
  font-family: "PT Sans Narrow", sans-serif;
  font-size: 20px;
}

#loading-content {
  display: block;
  position: relative;
  left: 50%;
  top: 50%;
  width: 170px;
  height: 170px;
  margin: -85px 0 0 -85px;
  border: 3px solid #f00;
}

#loading-content:after {
  content: "";
  position: absolute;
  border: 3px solid #0f0;
  left: 15px;
  right: 15px;
  top: 15px;
  bottom: 15px;
}

#loading-content:before {
  content: "";
  position: absolute;
  border: 3px solid #00f;
  left: 5px;
  right: 5px;
  top: 5px;
  bottom: 5px;
}

#loading-content {
  border: 3px solid transparent;
  border-top-color: #4d658d;
  border-bottom-color: #4d658d;
  border-radius: 50%;
  -webkit-animation: loader 2s linear infinite;
  -moz-animation: loader 2s linear infinite;
  -o-animation: loader 2s linear infinite;
  animation: loader 2s linear infinite;
}

#loading-content:before {
  border: 3px solid transparent;
  border-top-color: #d4cc6a;
  border-bottom-color: #d4cc6a;
  border-radius: 50%;
  -webkit-animation: loader 3s linear infinite;
  -moz-animation: loader 2s linear infinite;
  -o-animation: loader 2s linear infinite;
  animation: loader 3s linear infinite;
}

#loading-content:after {
  border: 3px solid transparent;
  border-top-color: #84417c;
  border-bottom-color: #84417c;
  border-radius: 50%;
  -webkit-animation: loader 1.5s linear infinite;
  animation: loader 1.5s linear infinite;
  -moz-animation: loader 2s linear infinite;
  -o-animation: loader 2s linear infinite;
}
@keyframes loader {
  0% {
    -webkit-transform: rotate(0deg);
    -ms-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    -ms-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
/* Transition for fade in and out of page views */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 2s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
