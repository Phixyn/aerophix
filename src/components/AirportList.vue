<template>
  <v-list id="feature-listing" two-line>
    <v-list-item-group
      active-class="pink--text"
    >
      <AirportListItem
        v-for="airport in airports"
        :key="airport.properties['gps_code']"
        :airport="airport"
      >
      </AirportListItem>
    </v-list-item-group>
  </v-list>
  <!-- </v-sheet> -->
</template>

<script>
import AirportListItem from "./AirportListItem.vue";

export default {
  name: "AirportList",
  components: { AirportListItem },
  props: ["airports"],
};
</script>

<style lang="scss">
#feature-listing {
  // TODO improve
  height: 93%;
  overflow: auto;
}

/* TODO move to AirportListItem component */
#feature-listing > * {
  display: block;
  margin: 0;
}

#feature-listing a {
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  color: #404;
  text-decoration: none;
}

#feature-listing a:last-child {
  border: none;
}

#feature-listing a:hover {
  background: #f0f0f0;
}
</style>
