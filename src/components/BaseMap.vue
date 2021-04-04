<template>
  <v-row class="fill-height no-gutters">
    <v-col id="map"></v-col>
    <div class="map-overlay">
      <fieldset>
        <input
          id="feature-filter"
          type="text"
          placeholder="Filter results by name"
        />
      </fieldset>
      <div id="feature-listing" class="listing">
        <a v-bind:key="feature.properties['iata_code']" v-for="feature in airports">{{feature.properties.name}} ({{feature.properties.abbrev}})</a>
      </div>
    </div>
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
      airports: []
    };
  },
  methods: {
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
    }
  },
  mounted() {
    mapboxgl.accessToken = this.accessToken;

    const map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/streets-v11",
      center: [-98, 38.88],
      maxZoom: 5,
      minZoom: 1,
      zoom: 3,
    });

    // Airport map icon tooltip
    const popup = new mapboxgl.Popup({
      closeButton: false,
    });

    map.on("load", () => {
      map.addSource("airports", {
        "type": "vector",
        "url": "mapbox://mapbox.04w69w5j",
      });

      map.addLayer({
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

      map.on("moveend", () => {
        const features = map.queryRenderedFeatures({ layers: ["airport"] });

        if (features) {
          const uniqueFeatures = this.getUniqueFeatures(features, "iata_code");
          // Populate features for the listing overlay
          //renderListings(uniqueFeatures);

          // Clear the input container
          //filterEl.value = "";

          // Store the current features in sn `airports` variable to
          // later use for filtering on `keyup`.
          this.airports = uniqueFeatures;
        }
      });

      map.on("mousemove", "airport", (evt) => {
        // Change the cursor style as a UI indicator
        map.getCanvas().style.cursor = "pointer";

        // Populate the popup and set its coordinates based on the feature
        const feature = evt.features[0];
        popup
          .setLngLat(feature.geometry.coordinates)
          .setText(feature.properties.name + " (" + feature.properties.abbrev + ")")
          .addTo(map);
      });

      map.on("mouseleave", "airport", () => {
        map.getCanvas().style.cursor = "";
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
  display: none;
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