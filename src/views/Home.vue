<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>{{ newPlaceParams }}</div>
    Name: <input type="text" v-model="newPlaceParams.name" /> Address:
    <input type="text" v-model="newPlaceParams.address" />
    <br />
    <button v-on:click="createPlace">Create Place</button>
    <div v-for="place in places" v-bind:key="place.id">
      <p>{{ place }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to the Places App!",
      places: [],
      newPlaceParams: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("http://localhost:3000/places").then((response) => {
        console.log(response.data);
        this.places = response.data;
      });
    },
    createPlace: function () {
      // foo
      console.log("test");
      var params = this.newPlaceParams;
      axios
        .post(`http://localhost:3000/places/`, params)
        .then((response) => {
          console.log(response.data);
          this.places.push(response.data);
          this.newPlaceParams.name = "";
          this.newPlaceParams.address = "";
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
