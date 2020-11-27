<template>
  <v-container>
    <v-row class="text-center">
      <v-col>
        <h2 class="display-2 font-weight-bold mb-3">Airports</h2>
      </v-col>
    </v-row>

    <v-row>
      <!-- TODO v-for, with a hardcoded list of ICAOS -->
      <v-col>
        <StationCard :station="info" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
import StationCard from "@/components/StationCard.vue";

export default {
  name: "HelloWorld",

  components: {
    StationCard,
  },
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
