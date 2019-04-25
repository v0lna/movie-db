<i18n>
{
  "ru": {
    "pageTitle": "Описание фильма:",
    "genre": "Жанр: "
  },
  "uk": {
    "pageTitle": "Опис фільму:",
    "genre": "Жанр: "

  },
  "en": {
    "pageTitle": "Film description:",
    "genre": "Genre: "
  }
}
</i18n>

<template>
  <div class>
    <div v-if="isLoading && isError === false" class="loader text-center min-w-screen flex">
      <div class="my-auto mx-auto">
        <h2 class >{{ $t("loadingMessage") }}</h2>
        <img class src="../img/5.svg" alt>
      </div>
    </div>
    <div v-else-if="isError" class="loader text-center min-w-screen flex">
      <div class="my-auto mx-auto">
        <h2 class>{{$t("error")}}{{error.status}}</h2>
        <h3>{{ $t("errorMessage") }}{{error.message}}</h3>
        <img class src="../img/sad.svg" alt>
      </div>
    </div>
    <div v-else>
      <div
        class="banner-image w-full bg-blue-light min-h-64 flex justify-center items-center text-2xl mb-4 p-8"
        :style="movieBanner"
      >
        <h1 class="movie-title text-white">{{ movie.title }}</h1>
      </div>
      <div class="flex flex-wrap xs:flex-no-wrap mx-8">
        <div class="w-full xs:w-1/4 xs:mr-4">
          <img :src="posterPath" alt>
        </div>
        <div class="w-full xs:w-3/4">
          <h2 >{{ $t("pageTitle") }}</h2>
          <p>
            {{ $t("genre") }}
            <router-link
              :to="`/genre/${genre.id}`"
              v-for="genre of movie.genres"
              :key="genre.id"
              class="list-reset"
            >{{`${genre.name} `}}</router-link>
          </p>
          <p>{{movie.overview}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import config from "@/config";

export default {
  props: ["lang"],
  name: "MoviePage",
  data() {
    return {
      movie: {},
      isLoading: true,
      isError: false,
      error: {}
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
    fetchData: async function(id = this.$route.params.id) {
      try {
        const res = await fetch(
          `https://api.themoviedb.org/3/movie/${id}?api_key=${config.api_key}&language=${this.lang}`
        );
        if (res.ok === false) {
          this.error.status = res.status;
          this.error.message = res.statusText;
          throw Error(res.status);
        }
        this.movie = await res.json();
        setTimeout(() => (this.isLoading = false), 450);
      } catch (error) {
        this.isLoading = false;
        this.isError = true;
        console.error(error);
      }
    }
  },
  async beforeRouteUpdate(to, from, next) {
    // this.isLoading = true;
    await this.fetchData(to.params.id);
    next();
  },
  watch: {
  lang(val) {
    this.fetchData();
    console.log(this.$i18n)

    // this.$i18n.locale = val
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
.loader {
  height: calc(100vh - 78px);
}
</style>
