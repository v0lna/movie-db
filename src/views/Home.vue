<i18n>
{
  "ru": {
    "pageTitle": "Самые популярные фильмы: "
  },
  "uk": {
    "pageTitle": "Найпопулярніші кінострічки: "
  },
  "en": {
    "pageTitle": "Most Popular Movies: "
  }
}
</i18n>

<template>
  <div class>
    <h1 class="mb-2 text-center">{{ $t('pageTitle') }}</h1>
    <MoviesList :movies="popularMovies"/>
  </div>
</template>

<script>
// @ is an alias to /src
import config from "@/config";
import MoviesList from "@/components/MoviesList";

export default {
  props: ["lang"],
  name: "Home",
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
          }&language=${this.lang}`
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
  },
  watch: {
    lang(val) {
      this.$i18n.locale = val
      this.fetchData();
      // console.log(val)
    }
  }
};
</script>

