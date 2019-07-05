<template>
  <div id="search-container">
    <SearchInput
      :searchTerm="searchTerm"
      @wordToSearch="searchDB($event)"
      @clearSearch="paginationFunction(...arguments)"
    />

    <b-container id="movie-list-container" class="mt-3" fluid>
      <b-row class="movie-list-container-row">
        <FilterButtons
          @selectedFilter="paginationFunction(...arguments)"
          :currentSearch="currentSearch"
        />
        <MovieList
          :selectedOption="selectedOption"
          :orderStatus="orderStatus"
          :orderAscDesc="orderAscDesc"
          :moviesArray="moviesArray"
          :moviesReturned="moviesReturned"
          @newMoviesArray="sortArrays( ...arguments)"
          :loading="loading"
          :apikey="apikey"
          :baseURL="baseURL"
          :current="current"
          :endpoint="endpoint"
        />
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

<script>
import SearchInput from "@/components/SearchInput.vue";
import FilterButtons from "@/components/FilterButtons.vue";
import MovieList from "@/components/MovieList.vue";
import { baseURL, apikey } from "@/assets/key.js";
import { setTimeout } from "timers";
export default {
  name: "search",
  props: {},
  components: {
    SearchInput,
    FilterButtons,
    MovieList
  },
  data() {
    return {
      apikey: apikey,
      baseURL: baseURL,
      selectedOption: "",
      searchTerm: "",
      endpoint: "popular",
      moviesArray: [],
      orderAscDesc: false,
      orderStatus: "Sort",
      loading: false,
      moviesReturned: true,
      pageNumbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      current: 1,
      currentSearch: ""
    };
  },
  //Watch for changes to movies array and rerender page
  watch: {
    moviesArray() {}
  },
  methods: {
    // Reusable AJAX call to the Movie DB with unique URL's passed in
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
        });
    },
    generateMovieList(passedEndpoint, pageNumber) {
      var endpointName;
      this.searchTerm = "";
      this.moviesArray = [];
      this.orderStatus = "Sort";
      this.orderAscDesc = false;
      this.moviesReturned = true;
      this.currentSearch = passedEndpoint;

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
    searchDB(wordToSearch) {
      this.searchTerm = wordToSearch;
      this.moviesArray = [];
      this.selectedOption = this.searchTerm;
      this.moviesReturned = false;
      this.endpoint = "search";
      // Start loading spinner
      setTimeout(() => {
        this.loading = true;
      }, 500);
      // Delay creation of URL to give user time to type search term
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
    sortArrays(arrayOrder, sortText) {
      this.moviesArray = [];
      this.orderStatus = sortText;
      this.moviesArray.push(arrayOrder[0]);
    }
  },
  mounted() {
    // Captures if users were on differet pagination page and goes back to same spot
    if (Object.keys(this.$attrs).length === 0) {
      this.paginationFunction(this.endpoint, 1);
    } else {
      this.paginationFunction(this.$attrs.endpoint, this.$attrs.current);
    }
  }
};
</script>


<style scoped>
#search-container {
  padding: 0 15px;
}

#movie-list-container {
  height: 79vh;
  overflow: scroll;
  margin-top: 10px !important;
}
.movie-list-container-row {
  height: 100%;
}

#pagination-container {
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
