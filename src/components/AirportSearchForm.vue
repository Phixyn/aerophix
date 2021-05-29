<template>
  <v-form
    id="airport-search-form"
    ref="search-form"
    class="grey lighten-2 px-4 my-auto mx-0"
  >
    <!-- TODO: Add clearable prop and handle clear event
            "click:clear" in the docs
    -->
    <!-- Emits custom input event for custom v-model binding.
      Note: event emitted MUST be called 'input'
      See: https://vuejs.org/v2/guide/components.html#Using-v-model-on-Components
    -->
    <v-text-field
      id="search-input"
      label="Search"
      placeholder="Filter airports by name"
      v-bind:value="value"
      v-on:input="$emit('input', $event)"
    ></v-text-field>
  </v-form> <!-- #airport-search-form -->
</template>

<script>
export default {
  name: "AirportSearchForm",

  // Prop for custom v-model binding
  props: ["value"],
};
</script>

<style lang="scss" scoped>
#airport-search-form {
  min-height: 5%;
  max-height: 15%;
}
</style>
