<template>
  <div class>
    <div v-if="isLoading" class="loader text-center min-w-screen flex">
      <div class="my-auto mx-auto">
        <h2 class>Is loading...</h2>
        <img class src="../img/5.svg" alt>
      </div>
    </div>
    <div v-else>
      <h1 class="mb-2 text-center">Самые популярные фильмы жанра {{genreName}}:</h1>
      <MoviesList :movies="genreMovies"/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import config from "@/config";
import MoviesList from "@/components/MoviesList";
import { Promise } from "q";

export default {
  name: "GenrePage",
  data() {
    return {
      genreMovies: [],
      genreName: "",
      isLoading: true
    };
  },
  methods: {
    fetchData() {
      Promise.all([
        fetch(
          `https://api.themoviedb.org/3/discover/movie?api_key=${
            config.api_key
          }&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page=1&with_genres=${
            this.$route.params.genreId
          }`
        )
          .then(genreResponse => {
            return genreResponse.json();
          })
          .then(genreData => {
            this.genreMovies = genreData.results;
          }),
        fetch(
          `https://api.themoviedb.org/3/genre/movie/list?api_key=${
            config.api_key
          }`
        )
          .then(res => {
            return res.json();
          })
          .then(json => {
            const genresArray = json.genres;
            genresArray.find(element => {
              if (element.id == this.$route.params.genreId) {
                this.genreName = element.name;
              }
            });
          })
      ]).then(() => {
        this.isLoading = false;
      });
    }
  },
  components: {
    MoviesList
  },
  created() {
    this.fetchData();
  }
};
</script>
<style>
.loader {
  height: calc(100vh - 78px);
}
</style>

