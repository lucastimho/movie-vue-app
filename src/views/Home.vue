<template>
  <div class="home">
    <h1>{{ message }}</h1>
    Title
    <input type="text" v-model="newMovieParams.title" />
    Year
    <input type="integer" v-model="newMovieParams.year" />
    Plot
    <input type="text" v-model="newMovieParams.plot" />
    <button v-on:click="createMovie()">Add Movie</button>
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
      <button v-on:click="showMovie(movie)">More Info</button>
    </div>
    <dialog id="movie-details">
      <form method="dialog">
        <h1>Movie Info:</h1>
        <p>
          Title:
          <input type="text" v-model="currentMovie.title" />
          <small>{{ currentMovie.title.length }} characters</small>
        </p>
        <p>
          Year:
          <input type="text" v-model="currentMovie.year" />
        </p>
        <p>
          Plot:
          <input type="text" v-model="currentMovie.plot" />
        </p>
        <p>
          English:
          <input type="text" v-model="currentMovie.english" />
        </p>
        <button v-on:click="updateMovie(currentMovie)">Update Movie</button>
        <button v-on:click="destroyMovie(currentMovie)">Delete</button>
        <button>close</button>
      </form>
    </dialog>
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
      newMovieParams: {},
      currentMovie: { title: "" },
      titleFilter: "",
      sortAttribute: "",
    };
  },
  created: function () {
    axios.get("http://localhost:3000/movies").then((response) => {
      this.movies = response.data;
    });
  },
  methods: {
    createMovie: function () {
      axios
        .post("http://localhost:3000/movies", this.newMovieParams)
        .then((response) => {
          console.log(response.data);
          this.products.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showMovie: function (movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    },
    updateMovie: function (movie) {
      axios.patch("http://localhost:3000/movies/" + movie.id, movie).then((response) => {
        console.log(response.data);
      });
    },
    destroyMovie: function (movie) {
      axios.delete("http://localhost:3000/movies/" + movie.id).then((response) => {
        console.log(response.data);
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    },
    setSortAttribute: function (inputAttribute) {
      this.sortAttribute = inputAttribute;
    },
  },
};
</script>
