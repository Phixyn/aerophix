<template>
  <v-app class="fill-height">
    <the-header></the-header>

    <!-- Sizes content based on application components -->
    <!-- TODO investigate if removing this has any effect -->
    <v-main class="fill-height">
      <!-- Provides the application the proper gutter -->
      <v-container
        fluid
        class="pa-0 fill-height"
      >
        <router-view></router-view>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import TheHeader from "@/components/TheHeader.vue";

export default {
  name: "App",
  components: {
    TheHeader
  },
};
</script>

<style lang="scss">
body {
  height: 100vh;
}
</style>