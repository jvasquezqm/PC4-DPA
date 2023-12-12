<!-- Archivo MovieItem.vue -->
<template>
  <q-card class="movie-card">
    <q-card-section>
      <q-img
        :src="'https://image.tmdb.org/t/p/w500/' + movie.backdrop_path"
      ></q-img>
    </q-card-section>
    <q-card-section>
      <!-- Título -->
      <q-card-title class="text-h6">{{ movie.title }}&nbsp;</q-card-title>
      <!-- Language -->
      <q-card-subtitle>
        <div>Language: {{ movie.original_language }}</div>
        <div>Release: {{ movie.release_date }}</div>
      </q-card-subtitle>
    </q-card-section>
    <q-card-section>
      <!-- Botón de favorito -->
      <q-btn
        icon-right="favorite"
        :label="isFavorite ? 'Quitar Favorito' : 'Agregar Favorito'"
        flat
        :color="isFavorite ? 'red' : 'black'"
        @click="toggleFavorite"
        class="cursor-pointer"
      >
      </q-btn>
    </q-card-section>
  </q-card>
</template>

<script>
import axios from "axios";

export default {
  name: "movieItem",
  props: {
    movie: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      isFavorite: false,
    };
  },
  async created() {
    // Al crear el componente, verifica si la película es un favorito
    this.isFavorite = await this.checkIfFavorite();
  },
  methods: {
    async toggleFavorite() {
      this.isFavorite = !this.isFavorite;
      await this.updateFavoriteStatus();
    },
    async updateFavoriteStatus() {
      // Enviar la información de favorito a la API
      try {
        const urlApi = "https://api.themoviedb.org/3/account/20708597/favorite";
        const mediaId = this.movie.id;

        // Construir el cuerpo del JSON
        const requestBody = {
          media_type: "movie",
          media_id: mediaId,
          favorite: this.isFavorite,
        };

        // Hacer la solicitud POST a la API
        const response = await axios.post(urlApi, requestBody, {
          headers: {
            Authorization:
              "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1NmY4NWI2ZWY4Y2VmMzg3MDg1ZGUyMDUyYmU1ODAxNiIsInN1YiI6IjY1NTU3MzEyNTM4NjZlMDEzOWUyMDQwYyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.FqPmYTqXFehkJPryZBqFWCYZCOpYaxsoXPzt9N-IXFU",
          },
          params: {
            api_key: "56f85b6ef8cef387085de2052be58016",
            // Asegúrate de incluir otros parámetros necesarios según la documentación de la API de TMDB
          },
        });

        console.log("Response data:", response.data);
      } catch (error) {
        console.error("Error al marcar/desmarcar favorito: ", error);
      }
    },
    async checkIfFavorite() {
      // Verificar si la película es un favorito
      try {
        const urlApi =
          "https://api.themoviedb.org/3/movie/" +
          this.movie.id +
          "/account_states";
        const response = await axios.get(urlApi, {
          headers: {
            Authorization:
              "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1NmY4NWI2ZWY4Y2VmMzg3MDg1ZGUyMDUyYmU1ODAxNiIsInN1YiI6IjY1NTU3MzEyNTM4NjZlMDEzOWUyMDQwYyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.FqPmYTqXFehkJPryZBqFWCYZCOpYaxsoXPzt9N-IXFU",
          },
          params: {
            api_key: "56f85b6ef8cef387085de2052be58016",
          },
        });

        return response.data.favorite;
      } catch (error) {
        console.error("Error al verificar favorito: ", error);
        return false;
      }
    },
  },
};
</script>

<style>
.movie-card {
  width: 300px; /* Tamaño fijo deseado */
  display: flex;
  flex-direction: column;
  height: 420px;
}
.cursor-pointer {
  cursor: pointer;
}
.movie-card q-card-section {
  height: 100px;
  overflow: hidden;
}
.image-section,
.title-section,
.date-section {
  height: 100px; /* Altura fija deseada */
  overflow: hidden;
}
</style>
