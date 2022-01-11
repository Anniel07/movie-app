
<template>
  <div>
    <div class="d-flex flex-row bg-dark">
      <div class="p-2 text-white"><h4>Movies</h4></div>
      <div class="p-2">
        <input
          v-model="search"
          v-on:keydown.enter.prevent="doSearch"
          id="search-input"
          class="form-control"
          type="search"
          placeholder="Search"
          aria-label="Search"
        />
      </div>
    </div>

    <div id="main-body">
      <div id="my-grid" class="row">
        <div
          v-for="movie in movies"
          :key="movie.id"
          class="col-md-4"
          @click="showDetails(movie)"
        >
          <img :src="baseUrl + movie.poster_path" :alt="movie.title" /> 

          <div class="footerImg">
            {{ movie.title }}
          </div>
        </div>
      </div>

      

      <div v-show="showModal">
        <movie-modal
          v-bind:movie="movie"
          v-bind:show="showModal"
          v-on:hide="showModal = false"
        />
      </div>

      <h2 v-if="movies.length == 0">No results found</h2>
      <div v-else class="overflow-auto">
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="20"
          aria-controls="my-grid"
          @change="doSearch"
          align="center"
        ></b-pagination>

        <!-- <p class="mt-3">Current Page: {{ currentPage }}</p> -->
      </div>
    </div>
  </div>
</template>



<script>
import Vue from "vue";
import axios from "axios";

import MovieModal from "./MovieModal.vue";

//const variables for make api request
const accessTok =
  "eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJmNWRlOGI4ZWQyYWMyYTE2MTUxOTQwZDUwY2Q0YjhhOSIsInN1YiI6IjYxZDkwNWU5NzJkODU1MDA5Nzg3MTEwYSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.uWFgLMDKWy8bjQy-RXa-3_PtFEkSB143h8xf4_d4mvM";
const config = {
  headers: {
    "Content-Type": "application/json;charset=utf-8",
    Authorization: "Bearer " + accessTok,
  },
};

export default {
  name: "MovieIndex",
  components: {
    MovieModal,
  },
  data: function () {
    return {
      movies: [],
      movie: {},
      showModal: false,
      search: "batman",
      //profilepic: null,
      baseUrl: "https://image.tmdb.org/t/p/w500",

      //variables for pagination
      perPage: 20,
      currentPage: 1,
      rows: 1,
    };
  },

  methods: {
    doSearch() {
      //console.log(this.search);
      var vm = this;
      axios
        .get(
          "https://api.themoviedb.org/3/search/movie?query=" +
            this.search +
            "&page=" +
            this.currentPage,
          config
        )
        .then((response) => {
          var mvs = response.data.results;
          Vue.set(vm, "movies", mvs);
          this.rows = response.data.total_results;
          //this.perPage = response.data.results.length;
          //console.log(this.currentPage);
        });
    },

    /**
     * Show the details for an specific movie
     */
    showDetails(movie) {
      var vm = this;
      axios
        .get("https://api.themoviedb.org/3/movie/" + movie.id, config)
        .then((response) => {
          var res = response.data;
          this.movie = res;
          this.showModal = true;
          //console.log(res);
        });

      vm.$bvModal.show("bv-modal-movie");
    },
  },
  
  mounted() {
    this.doSearch();
  },
};
</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#search-input {
  border-radius: 20px;
}

img {
  /*border: 4px solid #ccc;
  border-radius: 4px;
  */
  display: block;
  height: 300px;
  /*margin: 25px auto;*/
  width: 300px;
}

.footerImg {
  background-color: red;
  color: white;
  padding: 10px;
  width: 300px;
  margin-bottom: 40px;
}

#main-body {
  padding: 10px;
}
</style>
