<template>
  <v-app>
    <v-navigation-drawer app v-model="drawer" clipped>
      <v-container>
        <v-list-item>
          <v-btn text nuxt-link to="/toppage" class="m-5"
            ><v-icon>mdi-home</v-icon>HOME トップページ</v-btn
          >
        </v-list-item>
        <v-divider></v-divider>
        <v-list-item>
          <v-btn text nuxt-link to="/bmi" class="m-5"
            ><v-icon>mdi-calculator</v-icon>bmi指数</v-btn
          >
        </v-list-item>
        <v-list-item>
          <v-btn text nuxt-link to="/calendar" class="m-5"
            ><v-icon>mdi-calendar</v-icon>カレンダー</v-btn
          >
        </v-list-item>
        <v-list-item>
          <v-btn text nuxt-link to="/pomodoro" class="m-5"
            ><v-icon>mdi-timer</v-icon>カウントダウンタイマー</v-btn
          >
        </v-list-item>
      </v-container>
    </v-navigation-drawer>
    <v-app-bar color="#C0C0C0" dark app clipped-left>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>record</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-menu offset-y>
          <template v-slot:activator="{ on }">
            <v-btn v-on="on" text>Support<v-icon>mdi-menu-down</v-icon></v-btn>
          </template>
          <v-list dense nav>
            <v-subheader>Get help</v-subheader>
            <v-list-item>
              <v-list-item-content>
                <v-list-item>
                  <v-btn text nuxt-link to="/logout" class="m-5"
                    ><v-icon>mdi-logout</v-icon>ログアウト.退会</v-btn
                  >
                </v-list-item>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-menu>
      </v-toolbar-items>
    </v-app-bar>
    <Nuxt />
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: null
    };
  }
};
</script>
