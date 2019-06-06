<template>
  <div id="search-container">
    <b-jumbotron header="BootstrapVue" lead="Bootstrap 4 Components for Vue.js 2">
      <p>For more information visit website</p>
      <b-button variant="primary" href="#">More Info</b-button>
      <h6>{{ baseURL }}?{{ apikey }}</h6>
    </b-jumbotron>
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
      baseURL: baseURL
    };
  }
};
</script>

<style scoped>
#search-container {
  padding: 0 15px;
}
</style>
