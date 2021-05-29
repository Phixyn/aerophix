<template>
  <v-list id="airport-list" class="pa-0" two-line>
    <!-- class="fill-height"
      style="overflow: auto;" -->
    <v-list-item-group
      v-model="selected"
      active-class="pink--text"
    >
      <airport-list-item
        v-for="airport in airports"
        :key="airport.properties['gps_code']"
        :airport="airport"
      ></airport-list-item>
    </v-list-item-group>
  </v-list> <!-- #airport-list -->
</template>

<script>
import AirportListItem from "./AirportListItem.vue";

export default {
  name: "AirportList",

  components: {
    AirportListItem
  },

  props: [
    "airports"
  ],

  computed: {
    selectedAirport: function() {
      // TODO do the one liner thing Pog
      if (this.selected === null) {
        return null;
      }

      return this.airports[this.selected];
    }
  },

  data() {
    return {
      selected: null,
    };
  },

  watch: {
    selected: function (selectedAirport) {
      this.$emit('airport-selected', this.airports[selectedAirport]);
    }
  }
};
</script>

<style lang="scss" scoped>
#airport-list {
  // TODO improve
  height: 80%;
  overflow: auto;
  scrollbar-width: thin;
}
</style>
