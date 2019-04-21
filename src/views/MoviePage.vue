<template>
  <div class="home">
    <div
      class="banner-image w-full bg-blue-light h-64 flex justify-center items-center text-3xl mb-4"
      :style="movieBanner"
    >
      <h1 class="movie-title text-white">{{ movie.title }}</h1>
    </div>
    <img :src="posterPath" alt>
  </div>
</template>

<script>
// @ is an alias to /src
import config from "@/config";

export default {
  name: "home",
  data() {
    return {
      movie: {}
    };
  },
  created() {
    this.fetchData();
  },
  computed: {
    posterPath() {
      return `${config.images.secure_base_url}${config.images.poster_sizes[3]}${
        this.movie.poster_path
      }`;
    },
    movieBanner() {
      return {
        "background-image": `url(${config.images.secure_base_url}${
          config.images.backdrop_sizes[2]
        }${this.movie.backdrop_path})`
      };
    }
  },
  methods: {
    fetchData: async function() {
      try {
        const res = await fetch(
          `https://api.themoviedb.org/3/movie/${
            this.$route.params.id
          }?api_key=${config.api_key}`
        );
        this.movie = await res.json();
      } catch (error) {
        console.log(error);
      }
    }
  },
  components: {}
};
</script>

<style>
.banner-image {
  background-size: cover;
}
.movie-title {
  text-shadow: 7px 3px 14px black;
}
</style>
