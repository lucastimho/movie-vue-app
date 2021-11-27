<template>
  <div class="Moviesnew">
    <form v-on:submit.prevent="createMovie()">
      <h1>Create a Movie</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Title:</label>
        <textarea name="input" cols="30" rows="2" v-model="newMovieParams.title"></textarea>
      </div>
      <div>
        <label>Year:</label>
        <textarea name="input" cols="30" rows="2" v-model="newMovieParams.year"></textarea>
      </div>
      <div>
        <label>Plot:</label>
        <textarea name="input" cols="30" rows="5" v-model="newMovieParams.plot"></textarea>
      </div>
      <div>
        <label>English:</label>
        <textarea name="input" cols="30" rows="5" v-model="newMovieParams.english"></textarea>
      </div>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newMovieParams: {},
      errors: [],
      status: "",
    };
  },
  methods: {
    createMovie: function () {
      axios
        .post("http://localhost:3000/movies", this.newMovieParams)
        .then(() => {
          this.$router.push("/movies");
        })
        .catch((error) => {
          this.status = error.response.status;
          console.log(error.response);
        });
    },
  },
};
</script>
