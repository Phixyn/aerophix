<template>
  <div>
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

    <v-alert
      v-if="!isSearching && !mapHasAirports"
      dense
      text
      type="info"
      class="rounded-0"
    >
      Drag or zoom the map to populate results.
    </v-alert>

    <v-alert
      v-if="isSearching && !mapHasFilteredAirports"
      dense
      text
      type="warning"
      class="rounded-0"
    >
      No results found.
    </v-alert>

    <AirportList
      v-else-if="!isSearching"
      :airports="renderedAirports"
      v-on:airport-selected="$emit('airport-selected', $event)"
    />

    <AirportList
      v-else-if="isSearching && mapHasFilteredAirports"
      :airports="filteredAirports"
      v-on:airport-selected="$emit('airport-selected', $event)"
    />
  </div>
</template>

<script>
import AirportList from "./AirportList.vue";

export default {
  name: "AirportListSheet",
  components: {
    AirportList,
  },
  props: [
    "isSearching",
    "renderedAirports",
    "filteredAirports",
  ],
  data() {
    return {
      airportsFilter: "",
    };
  },
  computed: {
    mapHasAirports: function () {
      return this.renderedAirports.length > 0;
    },
    mapHasFilteredAirports: function() {
      return this.filteredAirports.length > 0;
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