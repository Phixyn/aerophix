<template>
  <v-app>
    <!-- TODO component -->
    <v-navigation-drawer v-model="showDrawer" app>
      <!--  -->
    </v-navigation-drawer>

    <!-- TODO put in a AppHeader component -->
    <v-app-bar app dense hide-on-scroll color="primary" dark>
      <v-app-bar-nav-icon @click="showDrawer = !showDrawer"></v-app-bar-nav-icon>

      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-toolbar-title class="text-h4">AeroPhix</v-toolbar-title>
      </div>

      <v-spacer></v-spacer>

      <v-btn text to="/">Home</v-btn>
      <v-btn text to="/about">About</v-btn>

      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <!-- Provides the application the proper gutter -->
      <v-container fluid class="pa-0 fill-height">
        <router-view></router-view>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({ showDrawer: true }),
};
</script>
