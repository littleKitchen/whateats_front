<template>
  <div class="geolocation">
    <h1>Current location is:</h1>
    <h2>Longitude:{{ formMess.longitude }}</h2>
    <h2>Latitude:{{ formMess.latitude }}</h2>
  </div>

  <form v-on:submit.prevent="submit()">
    <input
      placeholder="Radius"
      maxlength="15"
      type="text"
      v-model="formMess.radius"
    />
    <button type="submit" class="ui-button"><span>Submit</span></button>
  </form>
    <div class="result">
    <h1> The Result is:</h1>
    <h2>{{ formMess.rename }}</h2>
    <img v-bind:src="formMess.image_url" />
  </div>
</template>

<script>
import LocationSdk from "../commons/location-sdk";
import axios from "axios";

export default {
  name: "from-data",
  data() {
    return {
      formMess: {
        radius: "",
        latitude: "",
        longitude: "",
        rename: "",
        image_url: ""
      },
    };
  },
  methods: {
    submit() {
      axios.get(`http://localhost:3001/main/getResult?latitude=${this.formMess.latitude}&longitude=${this.formMess.longitude}&radius=${this.formMess.radius}`).then((response) => {
        this.formMess.rename = response.data.name;
        this.formMess.image_url = response.data.image_url;
      });

    },
  },
  mounted: function () {
    console.log("start to get location now.");
    let self = this;
    LocationSdk.getLocation({
      success: (res) => {
        self.formMess.latitude = res.latitude;
        self.formMess.longitude = res.longitude;
      },
      error: (res) => {
        console.log("failed to get location:" + res);
      },
    });
  },
};
</script>
