<template>
  <v-app>
    <!-- <v-row justify="center" align="start">
      <router-link class="mx-5" to="/">Home</router-link> |
      <router-link class="mx-5" to="/about">About</router-link>
    </v-row> -->

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  components: {
  },

  data: () => ({
    //
  }),
};
</script>
