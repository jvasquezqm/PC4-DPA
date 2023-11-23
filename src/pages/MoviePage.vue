  <!-- Archivo MoviePage.vue -->
  <template>

    <div class="movie-page">
        <div class="movie-list">
            <movieList />
        </div>
    </div>
  </template>

  <style>
  .movie-page {
    display: flex;
    justify-content: space-between;
  }

  /* .movie-filter {
    width: 25%;
  } */

  .movie-list {
    width: 100%;
  }
  </style>

  <script>
  import movieList
    from 'src/components/Movie/MovieList.vue'

  export default {
    name: "moviePage",
    components: {
        movieList
    },

  }


  </script>
