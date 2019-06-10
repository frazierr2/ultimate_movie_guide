<template>
  <div>
    <b-container id="detail-container">
      <b-row>
        <b-col class="movie-title">{{ movie.title }}</b-col>
      </b-row>
      <b-row>
        <b-col cols="7">
          <img class="movie-image" :src="'//image.tmdb.org/t/p/h632/'+ movie.backdrop_path">
        </b-col>
        <b-col cols="5">
          <h3>Movie Overview:</h3>
          <p>{{ movie.overview }}</p>
        </b-col>
      </b-row>
    </b-container>
    <h1 class="test"></h1>
  </div>
</template>

<script>
export default {
  name: "MovieDetails",
  data() {
    return {
      movie: this.$route.params.movie,
      id: this.$route.params.movie.id
    };
  },
  mounted() {}
};
</script>


<style scoped>
#detail-container {
  background-color: rgba(255, 255, 255, 0.8);
  padding: 10px;
}
.movie-title {
  font-size: 50px;
}
.movie-image {
  width: 100%;
  box-shadow: 5px 5px 10px black;
}
</style>
