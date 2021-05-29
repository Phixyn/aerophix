<template>
  <div class="fill-height" style="overflow: auto;">
    <airport-search-form
      v-model="airportsFilter"
      v-on:input="$emit('airport-search', $event)"
    ></airport-search-form>

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
import AirportSearchForm from "./AirportSearchForm.vue";
import TextAlert from "./TextAlert.vue";

export default {
  name: "AirportListView",

  components: {
    AirportList,
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

    /**
     * User has entered text into the input field of the airport search form.
     */
    listIsFiltered: function () {
      return this.airportsFilter !== "";
    },
  },
};
</script>
