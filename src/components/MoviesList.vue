<template>
  <div class="MoviesList mx-8">
    <MovieCard :movie="movie" v-for="(movie, index) of movies" :key="index" class="text-center"/>
  </div>
</template>

<script>
import MovieCard from "./MovieCard";
export default {
  props: ["movies"],
  name: "MoviesList",
  components: {
    MovieCard
  }
};
</script>

<style>
.MoviesList {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(205px, 1fr));
  grid-gap: 1rem;
}
</style>
