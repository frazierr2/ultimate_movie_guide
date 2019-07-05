<template>
  <b-col class="button-column" cols="2" md="1">
    <div class="button-rows">
      <object
        class="button-row-elements popular-btn"
        name="popular"
        @click="selectedFilterOption('popular',1)"
      >
        <font-awesome-icon :class="{currentSearch:'popular' == currentSearch}" icon="star" />
        <p>Popular</p>
      </object>
    </div>
    <div class="button-rows">
      <object
        class="button-row-elements top-rated-btn"
        name="top_rated"
        @click="selectedFilterOption('top_rated',1)"
      >
        <font-awesome-icon :class="{currentSearch:'top_rated' == currentSearch}" icon="chart-bar" />
        <p>Top Rated</p>
      </object>
    </div>
    <div class="button-rows">
      <object
        class="button-row-elements now-playing-btn"
        name="now_playing"
        @click="selectedFilterOption('now_playing',1)"
      >
        <font-awesome-icon
          :class="{currentSearch:'now_playing' == currentSearch}"
          icon="play-circle"
        />
        <p>Now Playing</p>
      </object>
    </div>
  </b-col>
</template>


<script>
export default {
  props: {
    currentSearch: {
      type: String
    }
  },
  data() {
    return {};
  },
  methods: {
    selectedFilterOption(endpoint, pageNumber) {
      this.$emit("selectedFilter", endpoint, pageNumber);
    }
  }
};
</script>


<style scoped>
.button-column {
  background-color: rgba(255, 255, 255, 0.7);
  border-right: 1px solid lightgrey;
  padding: 0;
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
.popular-btn:hover .fa-star,
.top-rated-btn:hover .fa-chart-bar,
.now-playing-btn:hover .fa-play-circle {
  color: #d4ac0d;
}
.currentSearch {
  color: rgb(56, 184, 131);
}
</style>
