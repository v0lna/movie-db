<i18n>
{
  "ru": {
    "searchText": "Поиск"
  },
  "uk": {
    "searchText": "Пошук"
  },
  "en": {
    "searchText": "Search"
  }
}
</i18n>
<template>
  <div>
    <div class="flex">
      <div class="relative mr-2">
        <input
          class="border px-4 py-2 rounded"
          type="text"
          name
          id
          v-model="searchText"
          @input="makeSearch"
          :placeholder="($t(`searchText`))"
          @keydown.down="downKeyHandler"
          @keydown.up="upKeyHandler"
          @keydown.enter="enterKeyHandler"
          @keydown.esc="escKeyHandler"
        >
        <div v-show="suggestions.length !== 0" class="absolute w-full mt-1">
          <ul class="list-reset bg-white shadow-lg border rounded overflow-hidden">
            <li
              class="p-2 hover:bg-grey-light"
              v-for="suggestion of suggestions"
              :key="suggestion.id"
              :class="[(higlightedItem === suggestions.indexOf(suggestion)) ? 'bg-grey' : '']"
            >
              <router-link :to="`/movie/${suggestion.id}`">{{suggestion.title}}</router-link>
            </li>
          </ul>
        </div>
      </div>
      <div class="ml-auto mr-2">
        <button type="button" class="px-4 py-2 bg-blue-dark text-white font-bold rounded">
          <router-link tag="span" :to="`/search/&query=${this.searchText}`">{{ $t("searchText") }}</router-link>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import config from "@/config.js";
export default {
  name: "SearchForm",
  props: ["lang"],
  data() {
    return {
      searchText: "",
      suggestions: [],
      higlightedItem: null
    };
  },
  methods: {
    downKeyHandler() {
      if (this.higlightedItem === null) {
        this.higlightedItem = 0;
      } else if (this.higlightedItem === this.suggestions.length - 1) {
        this.higlightedItem = 0;
      } else {
        this.higlightedItem++;
      }
    },
    upKeyHandler() {
      if (this.higlightedItem === null) {
        this.higlightedItem = this.suggestions.length - 1;
      } else if (this.higlightedItem === 0) {
        this.higlightedItem = this.suggestions.length - 1;
      } else {
        this.higlightedItem--;
      }
    },
    enterKeyHandler() {
      if (this.higlightedItem !== null) {
        this.$router.push(`/movie/${this.suggestions[this.higlightedItem].id}`);
        this.suggestions = [];
        this.searchText = "";
      } else {
        this.$router.push(`/search/&query=${this.searchText}`);
        this.suggestions = [];
        this.searchText = "";
      }
    },
    escKeyHandler() {
      this.suggestions = [];
      this.higlightedItem = null;
    },
    makeSearch: async function() {
      try {
        this.higlightedItem = null;
        if (this.suggestions.length !== 0) {
          this.suggestions = [];
        }
        if (this.searchText.length > 2) {
          const res = await fetch(
            `https://api.themoviedb.org/3/search/movie?api_key=${
              config.api_key
            }&query=${this.searchText}&language=${this.lang}`
          );
          const result = await res.json();

          this.suggestions = result.results.slice(0, 5);
        }
      } catch (error) {
        console.error(error);
      }
    }
  },
  watch: {
    $route() {
      this.suggestions = [];
      this.searchText = "";
    },
    lang(val) {
      this.makeSearch();
      // this.$i18n.locale = val
    }
  }
};
</script>

<style>
</style>
