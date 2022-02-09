<template>
  <!-- main div required by template-->
  <div>
    <div>
      <!--  div for handle navbar -->
      <b-navbar
        toggleable="lg"
        type="dark"
        variant="dark"
        fixed="top"
        style="padding-left: 15px"
      >
        <b-navbar-brand href="#">Movies</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-form>
              <b-form-input
                size="sm"
                style="border-radius: 15px; width: 80%"
                class="mr-sm-2"
                placeholder="Search"
                v-model="search"
                v-on:keydown.enter.prevent="doSearch()"
                id="search-input"
                autofocus
              ></b-form-input>
              <b-button
                size="sm"
                class="my-2 my-sm-0"
                @click.prevent="doSearch()"
              >
                <b-icon icon="search" variant="dark"></b-icon>
              </b-button>
            </b-nav-form>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>

    <!-- falta ver en rosolution whit hight monitor -->
    <div id="main-body">
      <div id="my-grid" class="row">
        <div v-for="movie in movies" :key="movie.id" class="col-md-4 bor">
          <div>
            <img
              @click="showDetails(movie)"
              :src="baseUrl + movie.poster_path"
              :alt="movie.title"
            />
            <div class="footerImg">
              {{ movie.title }}
            </div>
          </div>
        </div>
      </div>

      <!-- for show the modal with the movie details  -->
      <movie-modal v-bind:movie="movie" />

      <h2 class="text-success" v-if="emptySearch">
        Enter a text in the input and click search button
      </h2>
      <h2 class="text-danger" v-else-if="movies.length == 0">
        No results found
      </h2>
      <div v-else class="overflow-auto">
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="my-grid"
          @change="handlePageChange"
          align="center"
        ></b-pagination>

        <!-- Only for test -->
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
      emptySearch: true, //for handle text for show msg to users,
      //if the user don't type anything in the search input this vaule it's true otherwise false
      search: "", //string for search
      baseUrl: "https://image.tmdb.org/t/p/w500",

      //variables use for pagination
      perPage: 20,
      currentPage: 1,
      rows: 1, // total of results
    };
  },

  methods: {
    /**
     * listar all movies by query string
     * @param pageNum optional indicate the number of page to search
     */
    doSearch(pageNum = 1) {
      //console.log("Page " + pageNum)

      this.search = this.search.trim();
      if (this.search == "") {
        this.emptySearch = true;
        this.movies = []; //reset array
        return;
      }
      this.currentPage = pageNum; //update currentPage
      this.emptySearch = false;
      var vm = this;
      axios
        .get(
          "https://api.themoviedb.org/3/search/movie?query=" +
            this.search +
            "&page=" +
            pageNum,
          config
        )
        .then((response) => {
          var mvs = response.data.results;
          //console.log(mvs)
          Vue.set(vm, "movies", mvs);
          this.rows = response.data.total_results;
        });

      // give focus to search-input
      var inputText = document.getElementById("search-input");
      inputText.select();
      inputText.focus();
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
        });

      vm.$bvModal.show("bv-modal-movie");
    },

    handlePageChange(value) {
      this.doSearch(value);
    },
  },

  mounted() {
    this.doSearch();
  },
};
</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  display: block;
  height: 300px;
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
  margin: auto;
  margin-top: 4em;
}

.bor {
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Crea el efecto alrededor del grid */
img:hover {
  cursor: pointer;
  box-shadow: 0px 0px 35px rgb(100, 0, 0);
  border: 5px solid rgb(100, 0, 0);
}
</style>