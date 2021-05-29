<template>
  <div>
    <!-- TODO rename to AirportListView

        This can be a "view" instead, and views can be displayed on
        sheets?
    -->

    <text-alert v-if="!listIsFiltered && !mapHasAirports">
      {{ initialInfoMsg }}
    </text-alert>

    <text-alert v-if="listIsFiltered && !mapHasFilteredAirports" type="warning">
      {{ noResultsMsg }}
    </text-alert>

    <airport-list
      :airports="airports"
      v-on:airport-selected="$emit('airport-selected', $event)"
    ></airport-list>
  </div>
</template>

<script>
import AirportList from "./AirportList.vue";
import TextAlert from "./TextAlert.vue";

export default {
  name: "AirportListSheet",

  components: {
    AirportList,
    TextAlert,
  },

  props: [
    "renderedAirports",
    "filteredAirports",
    "listIsFiltered",
  ],

  data() {
    return {
      airportsFilter: "",
      initialInfoMsg: "Drag or zoom the map to populate results.",
      noResultsMsg: "No results found.",
    };
  },

  computed: {
    airports: function () {
      if (this.listIsFiltered && this.mapHasFilteredAirports) {
        return this.filteredAirports;
      }

      return this.renderedAirports;
    },

    mapHasAirports: function () {
      return this.renderedAirports.length > 0;
    },

    mapHasFilteredAirports: function () {
      return this.filteredAirports.length > 0;
    },
  },
};
</script>
