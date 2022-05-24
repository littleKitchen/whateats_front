<template>
  <form v-on:submit.prevent="submit()">
    <input
      placeholder="Radius"
      maxlength="15"
      type="text"
      v-model="formMess.radius"
    />
    <button type="submit" class="ui-button"><span>Submit</span></button>
  </form>
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
      },
    };
  },
  methods: {
    submit() {
      axios({
        baseURL: "http://localhost:3000/",
        method: "get",
        url: "main/getResult",
        data: this.formMess,
      }).then((res) => {
        console.log(res);
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
