<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col align="center">
            <v-card-actions>
              <v-btn @click="dialog = true" color="secondary" dark small fab
                ><v-icon>mdi-cog-outline</v-icon>
              </v-btn>
            </v-card-actions>
            <Pomodoro :dialog="dialog" :closeDialog="closeDialog" />
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Pomodoro from "~/components/Pomodoro";

export default {
  layout: "main",
  name: "App",

  components: {
    Pomodoro
  },

  data() {
    return {
      dialog: false
    };
  },
  methods: {
    closeDialog() {
      this.dialog = false;
    }
  }
};
</script>

<style scoped>
.v-col {
  position: relative;
  margin-top: 30px;
}
.v-btn {
  position: absolute;
  top: 20px;
}
</style>
