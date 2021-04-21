<template>
  <v-app>
    <v-app-bar color="#C0C0C0" dark app clipped-left>
      <v-app-bar-title>record</v-app-bar-title>
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
                  <v-btn text nuxt-link to="/toppage" class="m-5"
                    ><v-icon>mdi-home</v-icon>HOME トップページ</v-btn
                  >
                </v-list-item>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-menu>
      </v-toolbar-items>
    </v-app-bar>
    <v-main style="height: 600px;">
      <v-container text-center mt-5>
        <div>
          <span>現在登録されているメールアドレス</span>
          <div class="d-inline">
            <div class="my-5">
              {{ email }}
            </div>
          </div>
        </div>
        <v-btn @click="logOut">ログアウト</v-btn>
        <v-btn @click="unregister" class="red white--text ml-6">退会する</v-btn>
      </v-container>
    </v-main>
    <v-footer flat tile color="#C0C0C0" class="white--text text-center">
      <v-card-text mr-5>
        <img
          height="30"
          :src="require('@/assets/images/vuetify.svg')"
          class="mr-3"
        />
        <img
          height="30"
          :src="require('@/assets/images/nuxt.png')"
          class="mr-3"
        />
        <img
          height="30"
          :src="require('@/assets/images/firebase.png')"
          class="mr-3"
        />
        <img
          height="30"
          :src="require('@/assets/images/github.png')"
          class="mr-3"
        />
        <v-divider class="mt-3"></v-divider>
        <v-card-text class="fot white--text mt-2">
          {{ new Date().getFullYear() }} — <strong>record</strong>
        </v-card-text>
      </v-card-text>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  middleware: ["checkLogin"],
  data() {
    return {
      email: "",
      change: true
    };
  },
  methods: {
    changeEmail() {
      const user = this.$auth();
      user
        .updateEmail(this.email)
        .then(function() {
          this.email = "";
          alert("メールアドレスが正常に変更されました。");
        })
        .catch(function(error) {
          alert(
            "メールアドレスが正常に変更されませんでした。もう一度入力し直してください。"
          );
          this.email = "";
        });
    },
    passwordReset() {
      if (
        window.confirm(
          "現在登録されているメールアドレスにパスワード変更のメールを送信しますか？"
        )
      ) {
        const user = this.$fireAuth.currentUser;
        this.$fireAuth
          .sendPasswordResetEmail(user.email)
          .then(function() {
            // Email sent.
            alert("Email sent");
          })
          .catch(function(error) {
            // An error happened.
          });
      }
    },
    logOut() {
      if (window.confirm("ログアウトしますか？")) {
        this.$fireAuth.signOut().then(() => {
          alert("ログアウトしました。");
          this.$router.push("/");
        });
      }
    },
    async unregister() {
      const user = await this.$auth();
      if (user.uid === "ucHoVbAgvzeczk4O9gWX834vy9E3") {
        alert(
          "テストユーザーは退会出来ません。退会機能を試したい場合は、ユーザーを新規登録して行ってください。"
        );
        return;
      }
      if (window.confirm("本当に退会しますか？")) {
        await this.$fireAuth.currentUser.delete().then(() => {
          alert(
            "退会が完了しました。ユーザーに関わるすべてのデータを削除しました。"
          );
          this.$router.push("/");
        });
      }
    }
  },
  async created() {
    const user = await this.$auth();
    this.email = user.email;
  }
};
</script>