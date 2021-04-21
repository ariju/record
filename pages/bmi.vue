<template>
  <v-row>
    <v-col align="center" justify="center">
      <Bmi />
    </v-col>
  </v-row>
</template>

<script>
import Bmi from "~/components/Bmi";

export default {
  layout: "main",
  components: {
    Bmi
  }
};
</script>
