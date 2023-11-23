<!-- Archivo MovieList.vue -->
<template>
  <h6>Biblioteca de películas</h6>

    <div class="search-container">
      <q-input v-model="search" label="Buscar" @input="onSearchInput" />
    </div>

    <div class="button-container">
      <q-btn @click="showFavorites">Mis Favoritos</q-btn>
      <q-btn @click="getMovies">Mostrar Todo</q-btn>
    </div>
    <div class="movie-grid">
      <div v-for="movie in filteredMovies" :key="movie.id">
        <movieItem :movie="movie" />
      </div>
    </div>

  <div class="movie-list">
      <div class="movie-grid">
          <div class="movie-item" v-for="movie in movies" :key="movie.id">
              <movieItem :movie="movie" />
          </div>
      </div>
  </div>
</template>
<style>
.movie-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 20px;
}
.button-container {
  margin-bottom: 20px;
}
.search-container {
  margin-bottom: 20px;
}
</style>

<script>


import axios from 'axios'

import movieItem
  from 'components/Movie/MovieItem.vue'

export default {
  name: "movieList",
  components: {
      movieItem
  },
  mounted() {
      this.getMovies()
  },
  methods: {
      async getMovies() {
        try {
        var urlApi = 'https://api.themoviedb.org/3/discover/movie';
        var response = await axios.get(urlApi, {
          params: {
            api_key: '56f85b6ef8cef387085de2052be58016',
          },

        });
        console.log('Response data:', response.data);
        //this.datos = response.data.results; //
        //console.log(this.datos);
        this.movies = response.data.results;
        this.filteredMovies = [];
      } catch (error) {
        console.error('Error al obtener datos: ', error);
      }
    },

    async showFavorites() {
      // Filtrar películas favoritas
      this.filteredMovies = await this.getFavoriteMovies();
      this.movies =[];
    },
    async getFavoriteMovies() {
      try {
        const urlApi = 'https://api.themoviedb.org/3/account/20708597/favorite/movies';

        // Hacer la solicitud GET a la API de películas favoritas
        const response = await axios.get(urlApi, {
          headers: {
            Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1NmY4NWI2ZWY4Y2VmMzg3MDg1ZGUyMDUyYmU1ODAxNiIsInN1YiI6IjY1NTU3MzEyNTM4NjZlMDEzOWUyMDQwYyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.FqPmYTqXFehkJPryZBqFWCYZCOpYaxsoXPzt9N-IXFU',
          },
          params: {
            // Asegúrate de incluir otros parámetros necesarios según la documentación de la API de TMDB
          },
        });

        console.log('Favorite Movies:', response.data.results);
        return response.data.results;
      } catch (error) {
        console.error('Error al obtener películas favoritas: ', error);
        return [];
      }
    },
    async performSearch() {
      console.log('Performing search...');
      console.log('Search term:', this.search);

      // Simple search implementation
      this.filteredMovies = this.movies.filter(movie =>
        movie.title.toLowerCase().includes(this.search.toLowerCase())
      );

      console.log('Filtered Movies:', this.filteredMovies);
    },
    onSearchInput(event) {
      console.log('Input event:', event);
      console.log('Search term:', this.search);
    },
  },
  data() {
      return {
          movies: [],
          filteredMovies: [], // Agregamos una propiedad para las películas filtradas
          search: '',

      }
  }


}

</script>
