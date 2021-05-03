<template>
  <v-list id="airport-list" class="pa-0" two-line>
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
  </v-list> <!-- #airport-list -->
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
#airport-list {
  // TODO improve
  height: 90%;
  overflow: auto;
  scrollbar-width: thin;
}
</style>
