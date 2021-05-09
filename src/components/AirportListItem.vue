<template>
  <div class="list-item">
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title v-text="airport.properties['name']"></v-list-item-title>

        <v-list-item-subtitle
          class="text--primary"
          v-text="airport.properties['iata_code']"
        ></v-list-item-subtitle>

        <v-list-item-subtitle v-html="testSubtitle"></v-list-item-subtitle>

        <!-- <p class="mt-6"><v-btn>View &rarr;</v-btn></p> -->
      </v-list-item-content>

      <v-list-item-avatar
        tile
        size="96"
      >
        <v-img src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"></v-img>
      </v-list-item-avatar>
    </v-list-item>

    <v-divider></v-divider>
  </div> <!-- .list-item -->
</template>

<script>
export default {
  name: "AirportListItem",
  props: ["airport"],
  data() {
    return {
      testSubtitle: `${this.airport.properties["gps_code"]} | <a href="${this.airport.properties.wikipedia}" target="_blank">Wikipedia</a>`,
    };
  },
};
</script>

<style lang="scss">
.list-item a {
  text-decoration: none;
}

.list-item a:hover {
  background: #f0f0f0;
}
</style>
