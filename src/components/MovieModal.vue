<template>
  <div>
   <!--  <b-button id="show-btn" @click="$bvModal.show('bv-modal-example')"
      >Open Modal</b-button
    > -->

    <b-modal id="bv-modal-movie" hide-footer>
      <template #modal-title>
        <b>Movie Details</b>
      </template>
      <div class="d-block">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Field</th>
              <th>Value</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>adult</td>
              <td>{{ movie.adult ? "YES" : "No" }}</td>
            </tr>
            <tr>
              <td>backdrop path</td>
              <td>{{ movie.backdrop_path }}</td>
            </tr>
            <tr>
              <td>belongs to collection</td>
              <td>
                  {{ movie.belongs_to_collection != null ? movie.belongs_to_collection.name : 'No'}}
              </td>
            </tr>

            <tr>
              <td>budget</td>
              <td>{{ movie.budget }}</td>
            </tr>
            <tr>
              <td>Genres</td>
              <td>
                <ul>
                  <li v-for="genre in movie.genres" :key="genre.id">
                    {{ genre.name }}
                  </li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>homepage</td>
              <td>{{ movie.homepage }}</td>
            </tr>
            <tr>
              <td>id</td>
              <td>{{ movie.id }}</td>
            </tr>
            <tr>
              <td>imdb_id</td>
              <td>{{ movie.imdb_id }}</td>
            </tr>
            <tr>
              <td>original language</td>
              <td>{{ movie.original_language }}</td>
            </tr>
            <tr>
              <td>original title</td>
              <td>{{ movie.original_title }}</td>
            </tr>
            <tr>
              <td>overview</td>
              <td>{{ movie.overview }}</td>
            </tr>
            <tr>
              <td>popularity</td>
              <td>{{ movie.popularity }}</td>
            </tr>
            <tr>
              <td>poster path</td>
              <td>{{ movie.poster_path }}</td>
            </tr>
            <tr>
              <td>Production Companies</td>
              <td>
                <ul>
                  <li v-for="pc in movie.production_companies" :key="pc.id">
                    {{ pc.name }}
                  </li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>Production Countries</td>
              <td>
                <ul>
                  <li
                    v-for="pc in movie.production_countries"
                    :key="pc.iso_3166_1"
                  >
                    {{ pc.name }}
                  </li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>release date</td>
              <td>{{ movie.release_date }}</td>
            </tr>
            <tr>
              <td>revenue</td>
              <td>{{ movie.revenue }}</td>
            </tr>
            <tr>
              <td>runtime</td>
              <td>{{ movie.runtime }}</td>
            </tr>
            <tr>
              <td>spoken languages</td>
              <td>
                <ul>
                  <li
                    v-for="lang in movie.spoken_languages"
                    :key="lang.iso_639_1"
                  >
                    {{ lang.name }}
                  </li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>status</td>
              <td>{{ movie.status }}</td>
            </tr>
            <tr>
              <td>tagline</td>
              <td>{{ movie.tagline }}</td>
            </tr>
            <tr>
              <td>title</td>
              <td>{{ movie.title }}</td>
            </tr>
            <tr>
              <td>video</td>
              <td>{{ movie.video ? 'YES' : 'No'}}</td>
            </tr>
            <tr>
              <td>vote average</td>
              <td>{{ movie.vote_average }}</td>
            </tr>
            <tr>
              <td>vote count</td>
              <td>{{ movie.vote_count }}</td>
            </tr>
          </tbody>
        </table>
       
      </div>

      <b-button class="mt-3" block @click="$bvModal.hide('bv-modal-movie')"
        >Close</b-button
      >
    </b-modal>
  </div>
</template>


<script>
export default {
 
  props: {
    movie: {
      required: true,
      type: Object,
    },
    show: {
      default: false,
      required: false,
      type: Boolean,
    },
  }
};
</script>