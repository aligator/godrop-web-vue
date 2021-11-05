<template>
  <v-app>
    <v-app-bar
      app
      color="gray"
      dark
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
