<template>
  <div>
    <b-container id="detail-container" fluid>
      <div class="movie-detail-container">
        <b-row>
          <b-col class="movie-title" cols="12">{{ movieDetails[0].title }}</b-col>
          <b-col class="movie-tagline" cols="12">{{ movieDetails[0].tagline }}</b-col>
        </b-row>
        <b-row>
          <b-col cols="7">
            <img
              class="movie-image"
              :src="'//image.tmdb.org/t/p/h632/'+ movieDetails[0].backdrop_path"
            >
          </b-col>
          <b-col cols="5">
            <!-- Movie Overview/Details -->
            <div class="overview-container">
              <h3>Movie Overview</h3>
              <p>{{ movieDetails[0].overview }}</p>
            </div>
            <!-- Genres -->
            <b-row no-gutters>
              <b-col cols="12">
                <h3 class="small-heading">Genres</h3>
              </b-col>
              <b-col cols="12" style="margin-bottom:10px;">
                <b-row no-gutters>
                  <b-col v-for="genre in movieDetails[0].genres" :key="genre.id">{{ genre.name }}</b-col>
                </b-row>
              </b-col>
            </b-row>
            <!-- Release Date and Movie runtime -->
            <b-row no-gutters>
              <b-col>
                <h3 class="small-heading">Release Date</h3>
                <p class="small-text">{{ movieDetails[0].release_date }}</p>
              </b-col>
              <b-col>
                <h3 class="small-heading">Movie Runtime</h3>
                <p class="small-text">{{ movieDetails[0].runtime }} mins</p>
              </b-col>
            </b-row>
            <!-- Similar Movies -->
            <b-row no-gutters>
              <b-col cols="12">
                <h3 class="medium-heading">Similar Movies</h3>
              </b-col>
              <b-col
                v-for="similar in moviesSimilar"
                :key="similar.id"
                @click="getNewMovie(similar.id)"
              >
                <img :src="'//image.tmdb.org/t/p/w92/'+ similar.poster_path" alt>
                <p class="small-text">{{ similar.title }}</p>
              </b-col>
            </b-row>
          </b-col>
        </b-row>
      </div>
    </b-container>
  </div>
</template>

<script>
export default {
  name: "MovieDetails",
  data() {
    return {
      id: this.$route.params.movie.id,
      baseURL: this.$route.params.baseURL,
      apikey: this.$route.params.apikey,
      movieDetails: [],
      movieVideos: [],
      moviesSimilar: []
    };
  },
  methods: {
    getMovieDetails() {
      var url = `${this.baseURL}movie/${this.id}?api_key=${this.apikey}`;
      fetch(url, {
        method: "get"
      })
        .then(response => {
          return response.json();
        })
        .then(jsonData => {
          this.movieDetails.push(jsonData);
        });
    },
    getMovieVideos() {
      var url = `${this.baseURL}movie/${this.id}/videos?api_key=${this.apikey}`;
      fetch(url, {
        method: "get"
      })
        .then(response => {
          return response.json();
        })
        .then(jsonData => {
          this.movieVideos.push(jsonData.results[0]);
        });
    },
    getSimilarMovies() {
      var url = `${this.baseURL}movie/${this.id}/similar?api_key=${
        this.apikey
      }`;
      fetch(url, {
        method: "get"
      })
        .then(response => {
          return response.json();
        })
        .then(jsonData => {
          this.moviesSimilar.push(
            jsonData.results[0],
            jsonData.results[1],
            jsonData.results[2]
          );
        });
    },
    getNewMovie(key) {
      this.movieDetails = [];
      this.id = key;

      this.getMovieDetails();
    }
  },
  mounted() {
    this.getMovieDetails();
    this.getMovieVideos();
    this.getSimilarMovies();
  }
};
</script>


<style scoped>
#detail-container {
  margin-top: 25px;
  padding: 0 15px;
}
.movie-detail-container {
  background-color: rgba(255, 255, 255, 0.8);
  height: 85vh;
}
.movie-title {
  font-size: 50px;
  color: #5499c7;
}
.movie-tagline {
  margin-bottom: 12px;
  font-size: 15px;
  color: #808b96;
}
.movie-image {
  width: 97%;
  box-shadow: 5px 5px 10px black;
  padding: 7px;
}
.overview-container {
  padding: 0 20px 10px 20px;
}
.overview-container p {
  font-size: 15px;
}
.overview-container h3 {
  color: #04d277;
  font-weight: bolder;
}
.small-heading {
  color: #04d277;
  font-size: 18px;
}
.small-text {
  font-size: 13px;
}
.medium-heading {
  color: #e52313;
  font-size: 22px;
}
</style>
