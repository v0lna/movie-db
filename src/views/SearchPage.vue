<i18n>
{
  "ru": {
    "pageTitle": "Результаты поиска по запросу:"
  },
  "uk": {
    "pageTitle": "Результати пошуку за запитом:"
  },
  "en": {
    "pageTitle": "Search results for the query: "
  }
}
</i18n>
<template>
  <div class>
    <div v-if="isLoading" class="loader text-center min-w-screen flex">
      <div class="my-auto mx-auto">
        <h2 class>{{ $t("loadingMessage")}}</h2>
        <img class src="../img/5.svg" alt>
      </div>
    </div>
    <div v-else>
      <h1 class="mb-2 text-center">{{ $t("pageTitle") }} {{ $route.params.searchText }}</h1>
      <MoviesList :movies="searchMovies"/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import config from "@/config";
import MoviesList from "@/components/MoviesList";
import { Promise } from "q";

export default {
  props: ["lang"],
  name: "SearchPage",
  data() {
    return {
      searchMovies: [],
      genreName: "",
      isLoading: true
    };
  },
  methods: {
    fetchData(searchText = this.$route.params.searchText) {
      Promise.all([
        fetch(
          `https://api.themoviedb.org/3/search/movie?api_key=${
            config.api_key
          }&query=${searchText}&language=${this.lang}`
        )
          .then(searchResponse => {
            return searchResponse.json();
          })
          .then(searchData => {
            this.searchMovies = searchData.results;
          })
        // ,
        // fetch(
        //   `https://api.themoviedb.org/3/genre/movie/list?api_key=${
        //     config.api_key
        //   }&language=${this.lang}`
        // )
        //   .then(res => {
        //     return res.json();
        //   })
        //   .then(json => {
        //     const genresArray = json.genres;
        //     genresArray.find(element => {
        //       if (element.id == this.$route.params.genreId) {
        //         this.genreName = element.name;
        //       }
        //     });
        //   })
      ]).then(() => {
        setTimeout(() => {
          this.isLoading = false;
        }, 350);
      });
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
      this.fetchData();
      // this.$i18n.locale = val;
    }
  },
  async beforeRouteUpdate(to, from, next) {
    // this.isLoading = true;
    await this.fetchData(to.params.searchText);
    next();
  }
};
</script>
<style>
.loader {
  height: calc(80vh - 78px);
}
</style>

