<template>
  <div class="home">
    <h1>{{ message }}</h1>
    Name: <input type="text" v-model="newPlaceParams.name" /> Address:
    <input type="text" v-model="newPlaceParams.address" />
    <br />
    <button v-on:click="createPlace">Create Place</button>
    <div v-for="place in places" v-bind:key="place.id">
      <h1>{{ place.name }}</h1>
      <button v-on:click="showPlace(place)">Show Info</button>
    </div>
    <dialog id="place-details">
      <form method="dialog">
        <h1>Place Details</h1>
        <p>Name: <input type="text" v-model="currentPlace.name" /></p>
        <p>Address: <input type="text" v-model="currentPlace.address" /></p>
        <button v-on:click="updatePlace(currentPlace)">Update place</button>
        <button v-on:click="destroyPlace(currentPlace)">Destroy place</button>
        <button>Close</button>
      </form>
    </dialog>
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
      currentPlace: {},
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
    showPlace: function (place) {
      console.log(place);
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      console.log(place);
      var editPlaceParams = place;
      axios
        .patch(`http://localhost:3000/places/${place.id}`, editPlaceParams)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
    destroyPlace: function (place) {
      axios
        .delete(`http://localhost:3000/places/${place.id}`)
        .then((response) => {
          console.log("place is now gone, kaboom, ", response.data);
          var index = this.places.indexOf(place);
          this.places.splice(index, 1);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
