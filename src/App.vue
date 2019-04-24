<template>
  <div id="app">
    <div class="flex justify-between items-center">
      <div id="nav" class="text-center">
        <router-link to="/">Home</router-link>|
        <router-link to="/about">About</router-link>
      </div>
      <div class="flex items-center">
        <div>
          <select>
            <!-- <option label="Электроник" value="1" selected>Господин Электроник</option> -->
            <option label="Language" value="1" selected disabled>Language</option>
            <option>English</option>
            <option>Русский</option>
            <option>Українська</option>
          </select>
        </div>
        <SearchForm/>
      </div>
    </div>
    <router-view/>
  </div>
</template>


<script>
import SearchForm from "@/components/SearchForm";

export default {
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
