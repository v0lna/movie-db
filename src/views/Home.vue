<template>
  <div class>
    <h1 class="mb-2 text-center">Самые популярные фильмы:</h1>
    <MoviesList :movies="popularMovies"/>
  </div>
</template>

<script>
// @ is an alias to /src
import config from "@/config";
import MoviesList from "@/components/MoviesList";

export default {
  name: "home",
  data() {
    return {
      popularMovies: []
    };
  },
  methods: {
    fetchData: async function() {
      try {
        const res = await fetch(
          `https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=${
            config.api_key
          }`
        );
        const moviesData = await res.json();
        this.popularMovies = moviesData.results;
      } catch (error) {
        console.error(error);
      }
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
