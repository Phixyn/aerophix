<template>
  <!-- TODO change max-width (or dont even set one lol) -->
  <v-card class="mx-auto" max-width="400">
    <v-img
      class="white--text align-end"
      height="200px"
      src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
    >
      <v-card-title>ICAO</v-card-title>
    </v-img>

    <v-card-subtitle class="pb-0">IATA</v-card-subtitle>

    <v-card-text class="text--primary">
      <div>Airport Name</div>

      <div>City, ST, USA</div>
    </v-card-text>

    <v-card-actions>
      <v-btn color="orange" text>View</v-btn>
      <v-btn color="orange" text>Explore</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: "AirportInfoCard",
};
</script>
