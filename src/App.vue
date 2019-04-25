<template>
  <div id="app">
    <div class="flex justify-between items-center">
      <div id="nav" class="text-center">
        <router-link to="/">Home</router-link>|
        <router-link to="/about">About</router-link>
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
      selectedLanguage: "uk",
    };
  },
  components: {
    SearchForm
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
