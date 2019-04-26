<i18n>
{
  "ru": {
    "home": "Главная",
    "about": "О сайте"
  },
  "uk": {
    "home": "Головна",
    "about": "Про сайт"
  },
  "en": {
    "home": "Home",
    "about": "About"
  }
}
</i18n>
<template>
  <div id="app">
    <div class="flex justify-between items-center">
      <div id="nav" class="text-center">
        <router-link to="/">{{ $t("home") }}</router-link>|
        <router-link to="/about">{{ $t("about") }}</router-link>
      </div>
      <div>
        <select v-model="selectedLanguage">
          <option value="uk">Українська</option>
          <option value="ru">Русский</option>
          <option value="en">English</option>
        </select>
      </div>
      <SearchForm :lang="selectedLanguage"/>
    </div>
    <router-view :lang="selectedLanguage"/>
  </div>
</template>


<script>
import SearchForm from "@/components/SearchForm";

export default {
  name: "App",
  data() {
    return {
      selectedLanguage: "ru"
    };
  },
  components: {
    SearchForm
  },
  watch: {
    selectedLanguage(val) {
      this.$root.$i18n.locale = val;
      localStorage.setItem("lang", this.selectedLanguage);
      // console.log(this.$i18n);
    }
  },
  created() {
    if (localStorage.getItem("lang")) {
      this.selectedLanguage = localStorage.getItem("lang");
    }
  }
};
</script>


<style>
#app {
  /* font-family: "Avenir", Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  /* color: #2c3e50; */
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
