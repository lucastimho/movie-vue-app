<template>
  <div class="movies-show">
    <h3>{{ movie.title }}</h3>
    <p>{{ movie.year }}</p>
    <p>{{ movie.plot }}</p>
    <div>
      <router-link v-bind:to="`/movies/${movie.id}/edit`">Edit this movie</router-link>
      |
    </div>
    <router-link to="/movies">Back to all movies</router-link>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
    };
  },
  created: function () {
    axios.get("http://localhost:3000/movies/" + this.$route.params.id).then((response) => {
      this.movie = response.data;
    });
  },
  methods: {},
};
</script>
