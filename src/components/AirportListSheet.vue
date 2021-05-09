<template>
  <div>
    <!-- TODO move to its own component? Probs need to use slots
        SheetHeader component (or MapOverlayHeader?)
    -->
    <v-form
      id="airport-search-form"
      ref="search-form"
      class="grey lighten-2 px-4 my-auto mx-0"
    >
      <!-- TODO: Add clearable prop and handle clear event
            "click:clear" in the docs
      -->
      <v-text-field
        id="search-input"
        v-model="airportsFilter"
        label="Search"
        placeholder="Filter airports by name"
        v-on:input="$emit('input-change', $event)"
      ></v-text-field>
    </v-form> <!-- #airport-search-form -->

    <text-alert
      v-if="!listIsFiltered && !mapHasAirports"
      :text="initialInfoMsg"
    ></text-alert>

    <text-alert
      v-if="listIsFiltered && !mapHasFilteredAirports"
      :text="noResultsMsg"
      type="warning"
    ></text-alert>

    <!-- TODO could be fancy and potentially use only one AirportList here -->
    <airport-list
      v-else-if="!listIsFiltered && mapHasAirports"
      :airports="renderedAirports"
      v-on:airport-selected="$emit('airport-selected', $event)"
    ></airport-list>

    <airport-list
      v-else-if="listIsFiltered && mapHasFilteredAirports"
      :airports="filteredAirports"
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
  props: ["renderedAirports", "filteredAirports"],
  data() {
    return {
      airportsFilter: "",
      initialInfoMsg: "Drag or zoom the map to populate results.",
      noResultsMsg: "No results found.",
    };
  },
  computed: {
    mapHasAirports: function () {
      return this.renderedAirports.length > 0;
    },
    mapHasFilteredAirports: function () {
      return this.filteredAirports.length > 0;
    },
    /**
     * User has entered text into the input field to filter airports by
     * search term.
     */
    listIsFiltered: function () {
      return this.airportsFilter !== "";
    },
  },
};
</script>

<style lang="scss" scoped>
#airport-search-form {
  // TODO This does nothing lule
  min-height: 10%;
  height: 50%;
}
</style>
