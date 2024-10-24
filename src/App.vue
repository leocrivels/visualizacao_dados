<template>
  <v-app>
    <v-app-bar app color="primary" dark>

    </v-app-bar>

    <v-main>
      <ImagesFilter />
    </v-main>
  </v-app>
</template>

<script>
import ImagesFilter from './components/ImagesFilter.vue';

export default {
  name: 'App',

  components: {
    ImagesFilter,
  },

  data: () => ({
  }),
};
</script>
