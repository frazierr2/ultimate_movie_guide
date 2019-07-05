<template>
  <div>
    <font-awesome-icon class="search-icon" icon="search" />
    <input
      id="searchInput"
      class="mb-2 mr-sm-2 mb-sm-0"
      placeholder="Search Ultimate Movie Guide"
      v-model="userSearchTerm"
      @keydown.enter="searchDB"
    />
    <font-awesome-icon
      v-if="searchTerm.length"
      class="clear-icon"
      icon="times-circle"
      @click="clearSearch"
    />
  </div>
</template>

 
<script>
export default {
  name: "SearchInput",
  props: ["searchTerm"],
  data() {
    return {
      userSearchTerm: ""
    };
  },
  watch: {
    searchTerm(newValue) {
      this.userSearchTerm = newValue;
    }
  },
  methods: {
    searchDB() {
      this.$emit("wordToSearch", this.userSearchTerm);
    },
    clearSearch() {
      this.$emit("clearSearch", "popular", 1);
    }
  }
};
</script>



<style scoped>
#searchInput {
  width: 50%;
  padding-left: 30px;
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
</style>

