<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        />
      </v-col>

      <v-col class="mb-4">
        <h2 class="display-2 font-weight-bold mb-3">{{ info.icao }}</h2>
        <p class="lighten"></p>

        <p class="subheading font-weight-regular">
          {{ info.city }} // {{ info.country }}
        </p>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",

  data() {
    return {
      info: {},
      errors: [],
    };
  },
  async created() {
    await axios
      .get("http://localhost:5000/api/stations/KJFK")
      .then((response) => (this.info = response.data))
      .catch((err) => {
        this.errors.push(err);
      });
  },
};
</script>
