<template>
  <div id="search-container">
    <font-awesome-icon class="search-icon" icon="search"/>
    <input
      id="searchInput"
      class="mb-2 mr-sm-2 mb-sm-0"
      placeholder="Search Ultimate Movie Guide"
      v-model="searchTerm"
      v-on:keyup.enter="searchDB()"
    >
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
          <b-row>
            <b-col>
              <p
                class="selected-option-text"
                :selectedOption="selectedOption"
                col
              >{{ selectedOption }}</p>
            </b-col>
            <b-col class="sort-button">
              <p class>Sort</p>
            </b-col>
          </b-row>

          <b-row>
            <b-col v-for="(movie, index) in moviesArray[0]" :key="index" cols="3">
              <img
                class="movie-banner-picture"
                :src="'//image.tmdb.org/t/p/h632/'+ movie.poster_path"
                @click="getDetails(index)"
              >
              <p class="text-muted">{{ movie.original_title }}</p>
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
      moviesArray: []
    };
  },
  watch: {
    moviesArray() {}
  },
  methods: {
    generatePopularMovies() {
      this.searchTerm = "";
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
        });
    },
    generateTopRated() {
      this.searchTerm = "";
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
      this.searchTerm = "";
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
    },
    searchDB() {
      this.moviesArray = [];
      this.selectedOption = this.searchTerm;

      var encodedTerm = encodeURI(this.searchTerm),
        url = `${this.baseURL}search/movie?api_key=${
          this.apikey
        }&query=${encodedTerm}`;

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
    // Gets object by index and passes to detail view to display more data
    getDetails(index) {
      let movieDetails = this.moviesArray[0][index];

      this.$router.push({
        name: "movieDetails",
        params: {
          movie: movieDetails,
          baseURL: this.baseURL,
          apikey: this.apikey
        }
      });
    }
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
  padding-left: 30px;
}
#movie-list-container {
  /* border: 1px solid yellow; */
  height: 85vh;
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
.sort-button {
  text-align: right;
  top: 10px;
  right: 20px;
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
  box-shadow: 4px 4px 10px grey;
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
</style>
