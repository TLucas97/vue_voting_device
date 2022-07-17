<template>
  <v-app>
    <Header />
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
import Header from "./components/header.vue";

export default {
  components: {
    Header,
  },
  name: "App",

  data: () => ({
    //
  }),
};
</script>
