<template>
  <div class="movies-index">
    <div>
      Search by name:
      <input v-model="titleFilter" list="titles" />
      <datalist id="titles">
        <option v-for="movie in movies" v-bind:key="movie.id">{{ movie.title }}</option>
      </datalist>
    </div>
    <div>
      <button v-on:click="setSortAttribute('title')">Sort Alphabetically</button>
    </div>
    <div v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), sortAttribute)" :key="movie.id">
      {{ movie.title }}, {{ movie.year }},
      {{ movie.plot }}
      <router-link :to="`/movies/${movie.id}`">Read More</router-link>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      movies: [],
      titleFilter: "",
      sortAttribute: "",
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("http://localhost:3000/movies").then((response) => {
        this.movies = response.data;
        console.log("All Movies", this.movies);
      });
    },
  },
  setSortAttribute: function (inputAttribute) {
    this.sortAttribute = inputAttribute;
  },
};
</script>
