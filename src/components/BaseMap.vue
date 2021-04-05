<template>
  <v-row class="fill-height no-gutters">
    <v-col id="map"></v-col>
    <div class="map-overlay">
      <fieldset>
        <input
          v-model="airportsFilter"
          id="feature-filter"
          type="text"
          placeholder="Filter airports by name"
        />
      </fieldset>

      <p v-if="!isSearching && !showAirportList">
        Drag the map to populate results
      </p>

      <!-- <div id="feature-listing" class="listing"> -->
      <div v-if="!isSearching" id="feature-listing" class="listing">
        <a
          v-bind:key="airport.properties['iata_code']"
          v-for="airport in renderedAirports"
        >
          {{ airport.properties.name }} ({{ airport.properties.abbrev }})
        </a>
      </div> <!-- #feature-listing -->

      <p v-else-if="isSearching && filteredAirports.length === 0">
        No results found
      </p>

      <div v-else id="feature-listing" class="listing">
        <a
          v-bind:key="airport.properties['iata_code']"
          v-for="airport in filteredAirports"
        >
          {{ airport.properties.name }} ({{ airport.properties.abbrev }})
        </a>
      </div> <!-- #feature-listing -->
    </div> <!-- .map-overlay -->
  </v-row>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";

export default {
  name: "BaseMap",
  data() {
    return {
      accessToken: process.env.VUE_APP_MAPBOX_ACCESS_TOKEN,
      map: null,
      renderedAirports: [], // Airports currently visible on the map
      airportsFilter: "",
    };
  },
  computed: {
    showAirportList: function () {
      return this.renderedAirports.length > 0;
    },
    // TODO rename this when we move listings to separate component?
    // User has entered text into the input field
    isSearching: function () {
      return this.airportsFilter !== "";
    },
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
  },
  watch: {
    /**
     * Filter airports shown on the map based on the text entered on the
     * text input.
     */
    airportsFilter: function (newValue) {
      if (newValue === "") {
        this.map.setFilter("airport", ["has", "abbrev"]);
      }
      // else if (this.filteredAirports.length === 0) {
      //  TODO show no airports?
      // }
      else if (this.filteredAirports.length > 0) {
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
     * Normalizes a string by trimming whitespace and coverting it to lowercase.
     *
     * @param {string} string The string to normalize.
     */
    normalize(string) {
      return string.trim().toLowerCase();
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

    // Airport map icon tooltip
    const popup = new mapboxgl.Popup({
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
        const airports = this.map.queryRenderedFeatures({ layers: ["airport"] });

        if (airports) {
          const uniqueAirports = this.getUniqueFeatures(airports, "iata_code");
          // Populate features for the listing overlay
          //renderListings(uniqueFeatures);

          // Clear the text input field
          // this.airportsFilter = "";

          // Store the current features for later use for filtering
          this.renderedAirports = uniqueAirports;
        }
      });

      this.map.on("mousemove", "airport", (evt) => {
        // Change the cursor style as a UI indicator
        this.map.getCanvas().style.cursor = "pointer";

        // Populate the popup and set its coordinates based on the feature
        const feature = evt.features[0];
        popup
          .setLngLat(feature.geometry.coordinates)
          .setText(feature.properties.name + " (" + feature.properties.abbrev + ")")
          .addTo(this.map);
      });

      this.map.on("mouseleave", "airport", () => {
        this.map.getCanvas().style.cursor = "";
        popup.remove();
      });
    });
  },
};
</script>

<style lang="scss" scoped>
#map {
  position: absolute;
  left: 25%;
  top: 0;
  bottom: 0;
  width: 75%;
}

.map-overlay {
  position: absolute;
  width: 25%;
  top: 0;
  bottom: 0;
  left: 0;
  font: 12px/20px "Helvetica Neue", Arial, Helvetica, sans-serif;
  background-color: #fff;
  max-height: 100%;
  overflow: hidden;
}

.map-overlay fieldset {
  background: #ddd;
  border: none;
  padding: 10px;
  margin: 0;
}

.map-overlay input {
  display: block;
  border: none;
  width: 100%;
  border-radius: 3px;
  padding: 10px;
  margin: 0;
  box-sizing: border-box;
}

.map-overlay .listing {
  overflow: auto;
  max-height: 100%;
}

.map-overlay .listing > * {
  display: block;
  padding: 5px 10px;
  margin: 0;
}

.map-overlay .listing a {
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  color: #404;
  text-decoration: none;
}

.map-overlay .listing a:last-child {
  border: none;
}

.map-overlay .listing a:hover {
  background: #f0f0f0;
}
</style>