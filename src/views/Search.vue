<template>
  <div id="search-container">
    <input
      id="searchInput"
      class="mb-2 mr-sm-2 mb-sm-0"
      placeholder="Search Ultimate Movie Guide"
      v-model="searchTerm"
    >
    <!-- <button variant="primary" @click="searchDB">Search</button> -->

    <b-container id="movie-list-container" class="mt-3" fluid>
      <b-row class="movie-list-container-row">
        <b-col class="button-column" cols="1">
          <div class="button-rows">
            <div class="button-row-elements popular-btn" @click="generatePopularMovies()">
              <font-awesome-icon icon="star"/>
              <p>Popular</p>
            </div>
          </div>
          <div class="button-rows">
            <div class="button-row-elements top-rated-btn" @click="generateTopRated()">
              <font-awesome-icon icon="chart-bar"/>
              <p>Top Rated</p>
            </div>
          </div>
          <div class="button-rows">
            <div class="button-row-elements now-playing-btn" @click="generateNowPlaying()">
              <font-awesome-icon icon="play-circle"/>
              <p>Now Playing</p>
            </div>
          </div>
        </b-col>

        <b-col class="list-column" cols="11">
          <p class="selected-option-text" :selectedOption="selectedOption">{{ selectedOption }}</p>
          <b-row>
            <b-col v-for="popularItem in moviesArray[0]" :key="popularItem.id" cols="3">
              <img
                class="movie-banner-picture"
                :src="'//image.tmdb.org/t/p/h632/'+ popularItem.poster_path"
              >
              <p class="text-muted">{{ popularItem.original_title }}</p>
            </b-col>
          </b-row>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import { baseURL, apikey } from "@/assets/key.js";
export default {
  name: "search",
  props: {},
  data() {
    return {
      apikey: apikey,
      baseURL: baseURL,
      selectedOption: "",
      searchTerm: "",
      searchArray: [],
      popularArray: [],
      moviesArray: []
    };
  },
  watch: {
    moviesArray() {
      // console.log("The counter has changed!");
    }
  },
  methods: {
    generatePopularMovies() {
      this.moviesArray = [];
      this.selectedOption = "Popular Movies";
      var url = ` 
    ${this.baseURL}movie/popular?api_key=${this.apikey}&language=en-US
     `;
      fetch(url, {
        method: "get"
      })
        .then(response => {
          return response.json();
        })
        .then(jsonData => {
          this.moviesArray.push(jsonData.results);
          // this.popularArray.push(jsonData.results);
        });
    },
    generateTopRated() {
      this.moviesArray = [];
      this.selectedOption = "Top Rated";
      var url = ` 
    ${this.baseURL}movie/top_rated?api_key=${this.apikey}&language=en-US
     `;
      fetch(url, {
        method: "get"
      })
        .then(response => {
          return response.json();
        })
        .then(jsonData => {
          this.moviesArray.push(jsonData.results);
        });
    },
    generateNowPlaying() {
      this.moviesArray = [];
      this.selectedOption = "Now Playing";
      var url = ` 
    ${this.baseURL}movie/now_playing?api_key=${this.apikey}&language=en-US
     `;
      fetch(url, {
        method: "get"
      })
        .then(response => {
          return response.json();
        })
        .then(jsonData => {
          this.moviesArray.push(jsonData.results);
        });
    }
    // searchDB() {
    //   var encodedTerm = encodeURI(this.searchTerm),
    //     // url = `${this.baseURL}search/movie?api_key=${
    //     //   this.apikey
    //     // }&query=${encodedTerm}`;

    //   fetch(url, {
    //     method: "get"
    //   })
    //     .then(response => {
    //       return response.json();
    //     })
    //     .then(jsonData => {
    //       // this.bookResults = Object.values(jsonData.books);
    //       // this.searchArray = Object.values(jsonData.results);
    //       this.searchArray.push(Object.values(jsonData.results));
    //       // console.log(Object.values(jsonData.results));
    //     });
    // }
  },
  mounted() {
    this.generatePopularMovies();
  }
};
</script>

<style scoped>
#search-container {
  padding: 0 15px;
}
#searchInput {
  width: 50%;
}
#movie-list-container {
  border: 1px solid yellow;
  height: 80vh;
}
.movie-list-container-row {
  height: 100%;
}
.button-column {
  background: red;
  padding: 0;
}
.list-column {
  background: white;
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
  padding: 0 15px;
}
.selected-option-text {
  text-align: left;
  margin: 10px 0 10px 50px;
}

.popular-btn:hover .fa-star {
  color: gold;
}
.top-rated-btn:hover .fa-chart-bar {
  color: gold;
}
.now-playing-btn:hover .fa-play-circle {
  color: gold;
}
</style>
