<template>
  <v-app>
    <v-main>
      <MainComponent></MainComponent>
    </v-main>
  </v-app>
</template>

<script>
import MainComponent from './components/MainComponent.vue';

export default {
  name: 'App',

  components: {
    MainComponent,
  },

  data: () => ({
    //
  }),
};
</script>
