<template>
  <div id="search-container">
    <font-awesome-icon class="search-icon" icon="search" />
    <input
      id="searchInput"
      class="mb-2 mr-sm-2 mb-sm-0"
      placeholder="Search Ultimate Movie Guide"
      v-model="searchTerm"
      @keydown.enter="searchDB"
    />
    <font-awesome-icon
      v-if="searchTerm.length"
      class="clear-icon"
      icon="times-circle"
      @click="paginationFunction('popular', 1)"
    />

    <b-container id="movie-list-container" class="mt-3" fluid>
      <b-row class="movie-list-container-row">
        <b-col class="button-column" cols="2" md="1">
          <div class="button-rows">
            <object
              class="button-row-elements popular-btn"
              @click="paginationFunction('popular', 1)"
              name="popular"
            >
              <font-awesome-icon icon="star" />
              <p>Popular</p>
            </object>
          </div>
          <div class="button-rows">
            <object
              id="top_rated"
              class="button-row-elements top-rated-btn"
              @click="paginationFunction('top_rated', 1)"
              name="top_rated"
            >
              <font-awesome-icon icon="chart-bar" />
              <p>Top Rated</p>
            </object>
          </div>
          <div class="button-rows">
            <object
              class="button-row-elements now-playing-btn"
              @click="paginationFunction('now_playing', 1)"
              name="now_playing"
            >
              <font-awesome-icon icon="play-circle" />
              <p>Now Playing</p>
            </object>
          </div>
        </b-col>

        <b-col class="list-column" cols="10" md="11">
          <b-row>
            <b-col>
              <p
                class="selected-option-text"
                :selectedOption="selectedOption"
                col
              >{{ selectedOption }}</p>
            </b-col>
            <b-col class="sort-button">
              <span v-if="orderStatus !== 'Sort'">
                <font-awesome-icon class="sort-icon" v-if="!orderAscDesc" icon="long-arrow-alt-up" />
                <font-awesome-icon
                  class="sort-icon"
                  v-if="orderAscDesc"
                  icon="long-arrow-alt-down"
                />
              </span>

              <span
                @click="sortArrays(orderAscDesc = !orderAscDesc)"
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
      </b-row>
    </b-container>
    <div id="pagination-container">
      <b-button
        v-for="pageNumber in pageNumbers"
        :key="pageNumber"
        :value="pageNumber"
        squared
        @click="paginationFunction(endpoint, pageNumber)"
        :class="{current:pageNumber == current}"
      >{{ pageNumber }}</b-button>
    </div>
  </div>
</template>

// @click="generateMovieList(endpoint, pageNumber); current = pageNumber"

<script>
import { baseURL, apikey } from "@/assets/key.js";
import _ from "lodash";
import { setTimeout } from "timers";
export default {
  name: "search",
  props: {},
  data() {
    return {
      apikey: apikey,
      baseURL: baseURL,
      selectedOption: "",
      searchTerm: "",
      endpoint: "",
      moviesArray: [],
      orderAscDesc: false,
      orderStatus: "Sort",
      loading: false,
      moviesReturned: true,
      pageNumbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      current: 1
    };
  },
  //Watch for changes to movies array and rerender page
  watch: {
    moviesArray() {}
  },
  methods: {
    ajaxCall(url) {
      fetch(url, {
        method: "get"
      })
        .then(response => {
          this.moviesReturned = true;
          return response.json();
        })
        .then(jsonData => {
          this.moviesArray.push(jsonData.results);
          this.loading = false;
          // this.moviesReturned = true;
        });
    },
    generateMovieList(passedEndpoint, pageNumber) {
      var endpointName;
      this.searchTerm = "";
      this.moviesArray = [];
      this.orderStatus = "Sort";
      this.orderAscDesc = false;
      this.moviesReturned = true;
      // Capturing what name gets passed into function
      if (passedEndpoint == "popular") {
        endpointName = passedEndpoint;
        this.selectedOption = "Popular Movies";
      } else if (passedEndpoint == "top_rated") {
        endpointName = passedEndpoint;
        this.selectedOption = "Top Rated";
      } else {
        endpointName = passedEndpoint;
        this.selectedOption = "Now Playing";
      }
      // Setting endpoint value to be used on pagination
      this.endpoint = endpointName;
      // Generating URL
      var url = `
        ${this.baseURL}movie/${endpointName}?api_key=${this.apikey}&language=en-US&page=${pageNumber}
        `;

      this.ajaxCall(url);
    },
    searchDB() {
      this.moviesArray = [];
      this.selectedOption = this.searchTerm;
      this.moviesReturned = false;
      this.endpoint = "search";
      setTimeout(() => {
        this.loading = true;
      }, 500);

      setTimeout(() => {
        var encodedTerm = encodeURI(this.searchTerm),
          url = `${this.baseURL}${this.endpoint}/movie?api_key=${this.apikey}&query=${encodedTerm}`;

        if (this.searchTerm.length) {
          this.ajaxCall(url);
        } else {
          this.generateMovieList("popular", 1);
        }
      }, 2000);
    },
    paginationFunction(endpoint, pageNumber) {
      this.generateMovieList(endpoint, pageNumber);
      this.current = pageNumber;
    },
    // Gets object by index and passes to detail view to display more data
    getDetails(index) {
      let movieDetails = this.moviesArray[0][index];

      this.$router.push({
        name: "movieDetails",
        params: {
          movieID: movieDetails.id,
          baseURL: this.baseURL,
          apikey: this.apikey
        }
      });
    },
    sortArrays(arrayOrder) {
      var orderOfArray;
      if (arrayOrder == true) {
        this.orderStatus = "A-Z";
        orderOfArray = _.orderBy(this.moviesArray[0], "title", "asc");
      } else {
        orderOfArray = _.orderBy(this.moviesArray[0], "title", "desc");
        this.orderStatus = "Z-A";
      }
      this.moviesArray = [];
      this.moviesArray.push(orderOfArray);
    }
  },
  mounted() {
    this.paginationFunction("popular", 1);
  }
};
</script>



<style scoped>
#search-container {
  padding: 0 15px;
}
#searchInput {
  width: 50%;
  padding-left: 30px;
}
#movie-list-container {
  /* height: 85vh; */
  height: 79vh;
  overflow: scroll;
}
.movie-list-container-row {
  height: 100%;
}
.button-column {
  /* background: red; */
  background-color: rgba(255, 255, 255, 0.7);
  border-right: 1px solid lightgrey;
  padding: 0;
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
.list-column {
  /* background: white; */
  background-color: rgba(255, 255, 255, 0.7);
}
.search-icon {
  left: 23px;
  position: relative;
  color: lightgrey;
}
.clear-icon {
  position: relative;
  right: 33px;
  color: #b31b1e;
}

.button-rows {
  height: 100px;
  position: relative;
}
.button-row-elements {
  margin: 0;
  position: absolute;
  top: 50%;
  left: 50%;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  width: 100%;
}
.button-row-elements p {
  font-size: 15px;
}
.movie-banner-picture {
  width: 90%;
  height: 300px;
  margin: 5px 0;
  padding: 5px;
  box-shadow: 4px 4px 10px #566573;
}
.selected-option-text {
  text-align: left;
  margin: 10px 0 10px 50px;
}

.popular-btn:hover .fa-star {
  color: #f1c40f;
}
.top-rated-btn:hover .fa-chart-bar {
  color: #f1c40f;
}
.now-playing-btn:hover .fa-play-circle {
  color: #f1c40f;
}

#pagination-container {
  /* background-color: white; */
  margin-top: 5px;
  padding: 3px 0;
}
#pagination-container button {
  background: white;
  color: darkgrey;
}

.current {
  background-color: rgb(56, 184, 131) !important;
  color: white !important;
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

/* Responsive Media Queries */
@media only screen and (min-width: 768px) and (max-width: 991px) {
  .movie-banner-picture {
    height: 225px;
    width: 100%;
  }
  .text-muted {
    font-size: 12px;
  }
}
@media only screen and (min-width: 576px) and (max-width: 767px) {
  .movie-banner-picture {
    /* height: 225px; */
    width: 75%;
  }
  .text-muted {
    font-size: 12px;
  }
}
@media only screen and (max-width: 575px) {
  .movie-banner-picture {
    height: 250px;
    width: 60%;
  }
  #searchInput {
    font-size: 13px;
  }
}
</style>
