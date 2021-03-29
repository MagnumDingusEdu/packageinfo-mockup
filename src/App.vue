<template>
  <v-app >

    <v-main>
      <Mockup/>
    </v-main>
  </v-app>
</template>

<script>
import Mockup from "@/components/Mockup";

export default {
  name: 'App',

  components: {
    Mockup,
  },

  data: () => ({
    //
  }),
};
</script>
