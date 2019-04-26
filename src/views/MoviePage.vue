<i18n>
{
  "ru": {
    "pageTitle": "Описание фильма:",
    "genre": "Жанр: ",
    "budget": "Бюджет: ",
    "productionСompanies": "Компания производитель: "
  },
  "uk": {
    "pageTitle": "Опис фільму:",
    "genre": "Жанр: ",
    "budget": "Бюджет: ",
    "productionСompanies": "Компанія виробник: "

  },
  "en": {
    "pageTitle": "Film description:",
    "genre": "Genre: ",
    "budget": "Budget: ",
    "productionСompanies": "Production companies: "
  }
}
</i18n>

<template>
  <div class>
    <div v-if="isLoading && isError === false" class="loader text-center min-w-screen flex">
      <div class="my-auto mx-auto">
        <h2 class>{{ $t("loadingMessage") }}</h2>
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
          <h2 class="mb-2">{{ $t("pageTitle") }}</h2>
          <p class="mb-2">
            <span class="font-bold">{{ $t("budget") }}</span>
            {{ movie.budget }}$
          </p>

          <p class="mb-2">
            <span class="font-bold">{{ $t("genre") }}</span>
            <router-link
              :to="`/genre/${genre.id}`"
              v-for="(genre, index) of movie.genres"
              :key="genre.id"
              class="list-reset no-underline"
            >
              {{`${genre.name}`}}
              <span v-if="( movie.genres[index] !== (movie.genres.length - 1))">,</span>
            </router-link>
          </p>
          <p class="mb-2">{{movie.overview}}</p>
          <div class="mb-2">
            <span class="font-bold">{{ $t("productionСompanies") }}</span>
            <ul class="list-reset inline">
              <li
                class="inline mx-1"
                v-for="(company, index) of productiveCompany"
                :key="`${company.id}${company.name}`"
              >
                {{company.name}}
                <span
                  v-if="(productiveCompany[index] !== productiveCompany.length - 1)"
                >,</span>
              </li>
            </ul>
            <div class="my-1 flex flex-row justify-around items-center">
              <div
                v-if="company.logo_path != null"
                v-for="company of productiveCompany"
                :key="company.id"
              >
                <img class="w-ful self-center" :src="company.logo_path" alt>
              </div>
            </div>
          </div>
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
      error: {},
      productiveCompany: []
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
    },
    prodCompanyBanner() {
      // console.log(this.movie.production_companies.logo_path);
      // return `${config.images.secure_base_url}${config.images.logo_sizes[3]}${
      //   this.movie.production_companies[0].logo_path
      // }`;
      const array = [];
      this.movie.production_companies.forEach(obj => {
        const newLogo_path = `${config.images.secure_base_url}${
          config.images.logo_sizes[3]
        }${obj.logo_path}`;
        array.push({ ...obj, logo_path: newLogo_path });
      });
      console.log(array);
      // return `${config.images.secure_base_url}${config.images.logo_sizes[3]}${
      //   this.movie.production_companies.
      // }`;
    }
  },
  methods: {
    fetchData: async function(id = this.$route.params.id) {
      try {
        const res = await fetch(
          `https://api.themoviedb.org/3/movie/${id}?api_key=${
            config.api_key
          }&language=${this.lang}`
        );
        if (res.ok === false) {
          this.error.status = res.status;
          this.error.message = res.statusText;
          throw Error(res.status);
        }
        this.movie = await res.json();

        this.productiveCompany = [];
        this.movie.production_companies.forEach(obj => {
          let newLogo_path = null;
          if (obj.logo_path !== null) {
            newLogo_path = `${config.images.secure_base_url}${
              config.images.logo_sizes[2]
            }${obj.logo_path}`;
          }
          this.productiveCompany.push({ ...obj, logo_path: newLogo_path });
        });

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
      // console.log(this.$i18n);
      // this.$i18n.locale = val

      //  <div class="mb-2">
      //             <span class="font-bold">{{ $t("productionСompanies") }}</span>
      //             <table class>
      //               <tr
      //                 class="align-middle border-black border-2"
      //                 v-for="company of productiveCompany"
      //                 :key="company.id"
      //               >
      //                 <th>{{company.name}}</th>
      //                 <td>
      //                   <img v-if="company.logo_path != null" :src="company.logo_path" alt>
      //                 </td>
      //               </tr>
      //             </table>
      //           </div>
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
  height: calc(80vh - 78px);
}
</style>
