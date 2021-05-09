<template>
  <v-alert dense text :type="type" class="rounded-0">
    <slot></slot>
  </v-alert>
</template>

<script>
export default {
  name: "TextAlert",
  props: {
    type: {
      type: String,
      default: "info",
      validator: function (value) {
        // The value must match one of these strings
        return [
          "success",
          "info",
          "warning",
          "error"
        ].indexOf(value) !== -1;
      },
    },
  },
};
</script>
