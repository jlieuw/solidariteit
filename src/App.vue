<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >

        <span class="mr-2">Solidariteit</span>

      <v-spacer></v-spacer>

    </v-app-bar>

    <v-main>
      <Salaris/>
    </v-main>
  </v-app>
</template>

<script>
import Salaris from './components/Salaris';

export default {
  name: 'App',

  components: {
    Salaris,
  },

  data: () => ({
    //
  }),
};
</script>
