<template>
  <div class="list-item">
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title v-text="airport.properties['name']">
        </v-list-item-title>
        <v-list-item-subtitle v-text="airport.properties['iata_code']">
        </v-list-item-subtitle>

        <p>
          {{ airport.properties["gps_code"] }} |
          <a v-bind:href="airport.properties.wikipedia" target="_blank">
            Wikipedia
          </a>
        </p>

        <p class="mt-6"><v-btn>View &rarr;</v-btn></p>
      </v-list-item-content>

      <v-list-item-avatar>
        <v-img src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"></v-img>
      </v-list-item-avatar>
    </v-list-item>

    <v-divider></v-divider>
  </div>
  <!-- .list-item -->
</template>

<script>
export default {
  name: "AirportListItem",
  props: ["airport"],
};
</script>
