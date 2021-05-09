<template>
  <v-alert dense text :type="type" class="rounded-0">
    {{ text }}
  </v-alert>
</template>

<script>
export default {
  name: "TextAlert",
  props: {
    type: {
      type: String,
      default: "info",
    },
    text: {
      type: String,
      required: true,
    },
  },
};
</script>
