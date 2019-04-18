<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <p>{{movie.title}}</p>
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
  methods: {
    fetchData: async function() {
      try {
        const res = await fetch(
          `https://api.themoviedb.org/3/movie/550?api_key=${config.api_key}`
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
