<template>
  <v-app style="background-color: rgba(119, 178, 184, 0.253);">
    <v-app-bar
      app
      color="green"
      dark
      dense
    >
      <div class="d-flex align-center">
        GoDrop

        
      </div>

      <v-spacer></v-spacer>
  
    </v-app-bar>

    <v-main>
      <Main />
    </v-main>
  </v-app>
</template>

<script>
import Main from './components/main.vue'
export default {
  name: 'App',

  components: {
    Main
  },

  data: () => ({
    //
  }),
};
</script>
