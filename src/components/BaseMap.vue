<template>
  <div id="map" class="base-map"></div>
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

    new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/streets-v11",
      center: [-98, 38.88],
      maxZoom: 5,
      minZoom: 1,
      zoom: 3,
    });
  },
};
</script>

<style lang="scss" scoped>
#map {
  margin: 0;
  padding: 0;
  height: 100vh;
}
</style>
