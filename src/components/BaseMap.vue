<template>
  <v-row class="fill-height no-gutters">
    <v-col id="map"></v-col>
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
    };
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

      map.on("mousemove", "airport", (evt) => {
        // Change the cursor style as a UI indicator
        map.getCanvas().style.cursor = "pointer";

        // Populate the popup and set its coordinates based on the feature
        let feature = evt.features[0];
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
