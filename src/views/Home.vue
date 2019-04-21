<template>
  <div class>
    <h1 class="mb-2 text-center">Самые популярные фильмы:</h1>
    <MoviesList/>
  </div>
</template>

<script>
// @ is an alias to /src
import config from "@/config";
import MoviesList from "@/components/MoviesList";

export default {
  name: "home",

  components: {
    MoviesList
  }
};
</script>
