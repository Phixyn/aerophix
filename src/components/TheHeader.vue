<template>
  <v-app-bar
    app
    dense
    color="primary"
    dark
  >
    <div class="d-flex align-center">
      <v-img
        alt="Vuetify Logo"
        class="shrink mr-2"
        contain
        src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
        transition="scale-transition"
        width="40"
      ></v-img>

      <v-toolbar-title class="text-h4">AeroPhix</v-toolbar-title>
    </div>

    <v-spacer></v-spacer>

    <v-btn
      text
      to="/"
    >
      Home
    </v-btn>

    <v-btn
      text
      to="/about"
    >
      About
    </v-btn>

    <v-btn icon>
      <v-icon>mdi-heart</v-icon>
    </v-btn>
  </v-app-bar>
</template>

<script>
export default {
  name: "TheHeader",
};
</script>
