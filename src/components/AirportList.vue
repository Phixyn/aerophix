<template>
  <div
    id="list-sheet-container"
    class="fill-height"
  >
    <airport-search-form
      v-model="airportsFilter"
      v-on:input="$emit('airport-search', $event)"
    ></airport-search-form>

    <text-alert v-if="!listIsFiltered && !mapHasAirports">
      {{ initialInfoMsg }}
    </text-alert>

    <text-alert
      v-if="listIsFiltered && !mapHasFilteredAirports"
      type="warning"
    >
      {{ noResultsMsg }}
    </text-alert>

    <v-list
      v-if="listHasAirports"
      id="airport-list"
      class="pa-0"
      two-line
    >
      <v-list-item-group
        v-model="selectedAirport"
        active-class="pink--text"
      >
        <airport-list-item
          v-for="airport in airports"
          :key="airport.properties['gps_code']"
          :airport="airport"
        ></airport-list-item>
      </v-list-item-group>
    </v-list> <!-- #airport-list -->
  </div> <!-- #list-sheet-container -->
</template>

<script>
import AirportListItem from "./AirportListItem.vue";
import AirportSearchForm from "./AirportSearchForm.vue";
import TextAlert from "./TextAlert.vue";

export default {
  name: "AirportList",

  components: {
    AirportListItem,
    AirportSearchForm,
    TextAlert,
  },

  props: [
    "renderedAirports",
    "filteredAirports"
  ],

  data() {
    return {
      airportsFilter: "",
      selectedAirport: null,
      initialInfoMsg: "Drag or zoom the map to populate results.",
      noResultsMsg: "No results found.",
    };
  },

  computed: {
    airports: function () {
      if (this.listIsFiltered) {
        return this.mapHasFilteredAirports ? this.filteredAirports : {};
      }

      return this.renderedAirports;
    },

    mapHasAirports: function () {
      return this.renderedAirports.length > 0;
    },

    mapHasFilteredAirports: function () {
      return this.filteredAirports.length > 0;
    },

    listHasAirports: function () {
      return this.airports.length > 0;
    },

    /**
     * User has entered text into the input field of the airport search form.
     */
    listIsFiltered: function () {
      return this.airportsFilter !== "";
    },
  },

  watch: {
    selectedAirport: function (airport) {
      this.$emit("airport-selected", this.airports[airport]);
    },
  },
};
</script>

<style lang="scss" scoped>
#list-sheet-container {
  overflow: auto;
}

#airport-list {
  // TODO improve
  height: 85%;
  overflow: auto;
  scrollbar-width: thin;
}
</style>
