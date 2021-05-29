<template>
  <v-row class="fill-height" no-gutters>
    <v-col cols="3" class="fill-height">
      <airport-map-overlay>
        <template v-slot:header>
          <!-- TODO search form... -->
          <airport-search-form
            v-on:text-input="setAirportFilter"
          ></airport-search-form>
        </template>

        <template v-slot:default>
          <airport-list-sheet
            v-if="!hasAirportSelected"
            :renderedAirports="renderedAirports"
            :filteredAirports="filteredAirports"
            :listIsFiltered="listIsFiltered"
            v-on:airport-selected="selectAirport"
          ></airport-list-sheet>

          <!-- TODO header... MapOverlayHeader or SheetHeader -->
          <airport-info-card
            v-else
            v-on:navigate-back="unselectAirport"
          ></airport-info-card>
        </template>
      </airport-map-overlay> <!-- #map-overlay -->
    </v-col>

    <v-col id="map" class="fill-height" cols="9"></v-col>
  </v-row>
</template>

<script>
import AirportInfoCard from "./AirportInfoCard.vue";
import AirportListSheet from "./AirportListSheet.vue";
import AirportMapOverlay from "./AirportMapOverlay.vue";
import AirportSearchForm from './AirportSearchForm.vue';

import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";

export default {
  name: "AirportMap",

  components: {
    AirportMapOverlay,
    AirportListSheet,
    AirportInfoCard,
    AirportSearchForm,
  },

  data() {
    return {
      accessToken: process.env.VUE_APP_MAPBOX_ACCESS_TOKEN,
      map: null,
      // Airport map icon tooltip
      popup: null,
      // Airports currently visible on the map
      renderedAirports: [],
      airportsFilter: "",
      selectedAirport: {},
    };
  },

  computed: {
    // Airports that match the text entered in the text input
    filteredAirports: function () {
      if (this.airportsFilter === "") {
        return [];
      }

      const filterBy = this.normalize(this.airportsFilter);
      return this.renderedAirports.filter((airport) => {
        const name = this.normalize(airport.properties.name);
        const code = this.normalize(airport.properties.abbrev);
        return name.indexOf(filterBy) > -1 || code.indexOf(filterBy) > -1;
      });
    },

    hasAirportSelected: function () {
      return (
        this.selectedAirport && Object.keys(this.selectedAirport).length !== 0
      );
    },

    /**
     * User has entered text into the input field of the airport search form.
     */
    listIsFiltered: function () {
      return this.airportsFilter !== "";
    },
  },

  watch: {
    /**
     * Filter airports shown on the map based on the text entered on the
     * text input.
     */
    airportsFilter: function (newValue) {
      if (newValue !== "" && this.filteredAirports.length === 0) {
        // TODO show no airports?
        return false;
      } else if (newValue === "") {
        this.map.setFilter("airport", ["has", "abbrev"]);
      } else if (this.filteredAirports.length > 0) {
        this.map.setFilter("airport", [
          "match",
          ["get", "abbrev"],
          this.filteredAirports.map((feature) => {
            return feature.properties.abbrev;
          }),
          true,
          false,
        ]);
      }
    },
  },

  methods: {
    /**
     * TODO
     */
    setAirportFilter(filter) {
      this.airportsFilter = filter;
    },

    /**
     * TODO
     */
    selectAirport(airport) {
      this.selectedAirport = airport;
    },

    /**
     * TODO
     */
    unselectAirport() {
      this.selectedAirport = {};
    },

    /**
     * Normalizes a string by trimming whitespace and coverting it to lowercase.
     *
     * @param {string} string The string to normalize.
     */
    normalize(string) {
      return string.trim().toLowerCase();
    },

    /**
     * Highlight given airport feature on the map with a popup.
     */
    highlightMapAirport(airport) {
      this.popup
        .setLngLat(airport.geometry.coordinates)
        .setText(
          airport.properties.name + " (" + airport.properties.abbrev + ")"
        )
        .addTo(this.map);
    },

    /**
     * TODO
     *
     * Returns unique features in a given array of features.
     * Features come from tiled vector data, and feature geometries may be split
     * or duplicated across tile boundaries. As a result, features may appear
     * multiple times in query results.
     *
     * comparatorProperty can be any unique property of a feature. In this case,
     * `iata_code` is recommended.
     */
    getUniqueFeatures(features, comparatorProperty) {
      const existingFeatureKeys = {};
      const uniqueFeatures = features.filter((feature) => {
        if (existingFeatureKeys[feature.properties[comparatorProperty]]) {
          return false;
        } else {
          existingFeatureKeys[feature.properties[comparatorProperty]] = true;
          return true;
        }
      });

      return uniqueFeatures;
    },
  },

  mounted() {
    mapboxgl.accessToken = this.accessToken;

    this.map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/streets-v11",
      center: [-98, 38.88],
      maxZoom: 10,
      minZoom: 1,
      zoom: 3,
    });

    this.popup = new mapboxgl.Popup({
      closeButton: false,
    });

    this.map.on("load", () => {
      this.map.addSource("airports", {
        "type": "vector",
        "url": "mapbox://mapbox.04w69w5j",
      });

      this.map.addLayer({
        "id": "airport",
        "source": "airports",
        "source-layer": "ne_10m_airports",
        "type": "symbol",
        "layout": {
          "icon-image": "airport-15",
          "icon-padding": 0,
          "icon-allow-overlap": true,
        },
      });

      this.map.on("moveend", () => {
        const airports = this.map.queryRenderedFeatures({
          layers: ["airport"],
        });

        if (airports) {
          // Store the current features for later use for filtering
          this.renderedAirports = this.getUniqueFeatures(airports, "iata_code");
        }
      });

      this.map.on("mousemove", "airport", (evt) => {
        // Change the cursor style as a UI indicator
        this.map.getCanvas().style.cursor = "pointer";

        // Populate the popup and set its coordinates based on the feature
        const feature = evt.features[0];
        this.popup
          .setLngLat(feature.geometry.coordinates)
          .setText(
            feature.properties.name + " (" + feature.properties.abbrev + ")"
          )
          .addTo(this.map);
      });

      this.map.on("mouseleave", "airport", () => {
        this.map.getCanvas().style.cursor = "";
        this.popup.remove();
      });
    });
  },
};
</script>

<style lang="scss" scoped>
// #map {
//   position: absolute;
//   top: 0;
//   left: 25%;
//   bottom: 0;
//   width: 50%;
// }

// #map-overlay {
//   position: absolute;
//   top: 0;
//   left: 0;
//   bottom: 0;
//   width: 25%;
//   max-height: 100%;
//   overflow: hidden;
//   font: 12px/20px "Helvetica Neue", Arial, Helvetica, sans-serif;
// }
</style>
