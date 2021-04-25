<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
          <v-toolbar-title class=" ml-4">
            dip intern test front
          </v-toolbar-title>
      </div>
    </v-app-bar>

    <v-main>
      <Estimate/>
    </v-main>
  </v-app>
</template>

<script>
import Estimate from './components/Estimate';

export default {
  name: 'App',

  components: {
    Estimate,
  },

  data: () => ({
    //
  }),
};
</script>
