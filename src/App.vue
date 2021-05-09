<template>
  <v-app>
    <the-header></the-header>

    <v-main>
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
