<template>
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
      label="Search"
      placeholder="Filter airports by name"
      v-on:input="$emit('text-input', $event)"
    ></v-text-field>
  </v-form> <!-- #airport-search-form -->
</template>

<script>
export default {
  name: "AirportSearchForm",
};
</script>

<style lang="scss" scoped>
#airport-search-form {
  // TODO This does nothing lule
  background-color: red;
  min-height: 10%;
  height: 50%;
}
</style>
