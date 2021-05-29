<template>
  <!-- TODO needs fill-height -->
  <div>
    <!-- TODO move to a SheetHeader or something component -->
    <v-sheet class="sheet-header grey lighten-2 px-4 py-2">
      <v-btn
        icon
        title="Back"
        v-on:click="$emit('navigate-back')"
      >
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
    </v-sheet>

    <v-card
      class="mx-auto"
      flat
      rounded="0"
    >
      <v-img
        class="white--text align-end"
        height="200px"
        src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
      >
        <v-card-title>ICAO</v-card-title>
      </v-img>

      <v-card-subtitle class="pb-0">IATA</v-card-subtitle>

      <v-card-text class="text--primary">
        <div>Airport Name</div>

        <div>City, ST, USA</div>
      </v-card-text>

      <v-card-actions>
        <v-btn
          text
          color="orange"
        >
          View
        </v-btn>

        <v-btn
          text
          color="orange"
        >
          Set as current
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "AirportDetailedInfo",
};
</script>

<style lang="scss" scoped>
.sheet-header {
  // TODO
}
</style>