<template>
  <div>
    <router-link class="block" :to="moviePath">
      <img :src="posterPath" alt>
      <h3>{{movie.title}}</h3>
    </router-link>
  </div>
</template>

<script>
import config from "@/config";

export default {
  props: ["movie"],

  computed: {
    posterPath() {
      return `${config.images.secure_base_url}${
        config.images.backdrop_sizes[0]
      }${this.movie.poster_path}`;
    },
    moviePath() {
      return `/movie/${this.movie.id}`;
    }
  }
};
</script>

<style>
</style>
