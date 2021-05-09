<template>
  <v-app>
    <app-header></app-header>

    <v-main>
      <!-- Provides the application the proper gutter -->
      <v-container fluid class="pa-0 fill-height">
        <router-view></router-view>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import AppHeader from "@/components/AppHeader.vue";

export default {
  name: "App",
  components: { AppHeader },
};
</script>
