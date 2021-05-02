<template>
  <v-sheet id="feature-listing">
    <v-list>
      <AirportListItem
        v-for="airport in airports"
        :key="airport.properties['gps_code']"
        :airport="airport"
      >
      </AirportListItem>
    </v-list>
  </v-sheet>
</template>

<script>
import AirportListItem from "./AirportListItem.vue";

export default {
  name: "AirportList",
  components: { AirportListItem },
  props: ["airports"],
};
</script>

<style lang="scss" scoped>
#feature-listing {
  overflow: auto;
  max-height: 100%;
}

#feature-listing > * {
  display: block;
  padding: 5px 10px;
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
