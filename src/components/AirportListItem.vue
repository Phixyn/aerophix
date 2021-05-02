<template>
  <v-list-item>
    <v-list-item-content>
      <v-img
        class="white--text align-end"
        height="200px"
        src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
      >
      </v-img>
      <v-divider></v-divider>

      <v-list-item-title v-text="airport.properties['gps_code']">
      </v-list-item-title>
      <v-list-item-subtitle v-text="airport.properties['gps_code']">
      </v-list-item-subtitle>
      <a v-bind:href="airport.properties.wikipedia" target="_blank">
        Wikipedia
      </a>
    </v-list-item-content>
  </v-list-item>
</template>

<script>
export default {
  name: "AirportListItem",
  props: ["airport"],
};
</script>
