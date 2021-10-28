<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="@/assets/logo.svg"
          transition="scale-transition"
          width="40"
        />

        ncRNA Netowrk
      </div>

      <v-spacer></v-spacer>
      <v-btn href="/" class="ma-1" plain> Home </v-btn>
      <v-btn href="/network" class="ma-1" plain> Network Analysis </v-btn>
      <v-btn href="/about" class="ma-1" plain> About </v-btn>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>
