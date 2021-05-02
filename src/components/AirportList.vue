<template>
  <v-sheet>
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
