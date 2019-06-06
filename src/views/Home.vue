<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <LandingPage msg="test to Your Vue.js ApNOp"/>
  </div>
</template>

<script>
// @ is an alias to /src
import LandingPage from "@/components/LandingPage.vue";

export default {
  name: "home",
  components: {
    LandingPage
  }
};
</script>
