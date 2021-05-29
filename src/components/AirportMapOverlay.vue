<template>
  <v-sheet
    id="map-overlay"
    class="fill-height"
  >
    <header id="map-overlay-header">
      <slot name="header"></slot>
    </header>

    <!-- Main content -->
    <article
      id="map-overlay-content"
      class="fill-height"
    >
      <slot></slot>
    </article>
  </v-sheet> <!-- #map-overlay-content -->
</template>

<script>
export default {
  name: "AirportMapOverlay",
};
</script>

<style lang="scss">
#map-overlay {
  // position: absolute;
  // top: 0;
  // left: 0;
  // bottom: 0;
  // width: 50%;
  // max-height: 100%;
  overflow: auto;
  // font: 12px/20px "Helvetica Neue", Arial, Helvetica, sans-serif;
}

#map-overlay-header {
  // TODO
}

#map-overlay-content {
  overflow: auto;
}
</style>
