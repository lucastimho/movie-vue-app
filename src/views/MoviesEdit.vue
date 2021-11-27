<template>
  <div class="moviesedit">
    <form v-on:submit.prevent="updateMovie()">
      <h1>Edit a Movie</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Title:</label>
        <textarea name="input" cols="30" rows="2" v-model="currentMovieParams.title"></textarea>
      </div>
      <div>
        <label>Year:</label>
        <textarea name="input" cols="30" rows="2" v-model="currentMovieParams.year"></textarea>
      </div>
      <div>
        <label>Plot:</label>
        <textarea name="input" cols="30" rows="6" v-model="currentMovieParams.plot"></textarea>
      </div>
      <input type="submit" value="Submit" />
    </form>
    <button v-on:click="deleteMovie()">Delete</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      currentMovieParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`http://localhost:3000/movies/${this.$route.params.id}`).then((response) => {
      console.log("Movie Info:", response.data);
      this.currentMovieParams = response.data;
    });
  },
  methods: {
    updateMovie: function () {
      axios
        .patch(`http://localhost:3000/movies/${this.$route.params.id}`, this.currentMovieParams)
        .then((response) => {
          this.$router.push(`http://localhost:3000/movies/${response.data.id}`);
        })
        .catch((error) => console.log(error.response));
    },
    deleteMovie: function () {
      axios.delete(`http://localhost:3000/movies/${this.$route.params.id}`).then((response) => {
        console.log(response.data);
        this.$router.push("http://localhost:3000/movies");
      });
    },
  },
};
</script>
