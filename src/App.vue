<template>
  <v-app>
    <cats />
  </v-app>
</template>

<script>
import cats from "@/components/cats.vue";
export default {
  name: "App",

  components: {
    cats,
  },

  data: () => ({}),
};
</script>
<style>
</style>
