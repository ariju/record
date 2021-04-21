<template>
  <v-app>
    <v-app-bar color="#C0C0C0" dark app clipped-left>
      <v-app-bar-title>record</v-app-bar-title>
    </v-app-bar>
    <v-main>
      <v-container style="max-width: 500px">
        <v-card tile elevation="0" background-color="white">
          <v-tabs v-model="tab" grow color="primary">
            <v-tab> ログイン </v-tab>
            <v-tab> 新規登録 </v-tab>
          </v-tabs>

          <!-- ログイン画面 -->
          <v-tabs-items v-model="tab">
            <v-tab-item class="white">
              <v-card-actions>
                <v-form v-model="isValid.login">
                  <v-row class="text-center">
                    <v-col cols="12">
                      <v-text-field
                        v-model="login.email"
                        outlined
                        label="Email"
                        color="info"
                        :rules="[rules.required, rules.email]"
                        validate-on-blur
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field
                        type="password"
                        v-model="login.password"
                        outlined
                        label="Password"
                        placeholder="6文字以上24文字以下"
                        color="info"
                        :rules="[rules.required, rules.password]"
                        validate-on-blur
                      ></v-text-field>
                    </v-col>
                    <v-col>
                      <v-btn
                        @click="logIn"
                        width="30%"
                        :disabled="!isValid.login"
                        >Login</v-btn
                      >
                    </v-col>
                  </v-row>
                </v-form>
              </v-card-actions>
              <v-card-text>
                テストユーザー（クリックで簡易ログイン）
                <v-simple-table class="blue-grey lighten-5">
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th class="text-left">TestUser</th>
                        <th class="text-left">Email</th>
                        <th class="text-left">Password</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        v-for="item in testUsers"
                        :key="item.email"
                        @click="testLogin(item)"
                      >
                        <td>{{ item.user }}</td>
                        <td>{{ item.email }}</td>
                        <td>{{ item.password }}</td>
                      </tr>
                    </tbody>
                  </template>
                </v-simple-table>
              </v-card-text>
            </v-tab-item>

            <!-- 新規登録画面 -->
            <v-tab-item class="white">
              <v-card-actions>
                <v-form v-model="isValid.signup">
                  <v-row class="text-center">
                    <v-col cols="12">
                      <v-text-field
                        v-model="signup.nickname"
                        outlined
                        label="Nickname"
                        color="info"
                        :rules="[rules.required, rules.name]"
                        validate-on-blur
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field
                        v-model="signup.email"
                        outlined
                        label="Email"
                        color="info"
                        :rules="[rules.required, rules.email]"
                        validate-on-blur
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field
                        type="password"
                        v-model="signup.password"
                        outlined
                        label="Password"
                        placeholder="6文字以上24文字以下"
                        color="info"
                        :rules="[rules.required, rules.password]"
                        validate-on-blur
                      ></v-text-field>
                    </v-col>
                    <v-col>
                      <v-btn
                        @click="signUp"
                        width="30%"
                        :disabled="!isValid.signup"
                        >SignUp</v-btn
                      >
                    </v-col>
                  </v-row>
                </v-form>
              </v-card-actions>
            </v-tab-item>
          </v-tabs-items>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      tab: null,
      login: {
        email: "",
        password: ""
      },
      signup: {
        nickname: "",
        email: "",
        password: ""
      },
      testUsers: [
        {
          user: "テストユーザー",
          email: "test@gmail.com",
          password: "123456"
        }
      ],
      isValid: {
        login: false,
        signup: false
      },
      rules: {
        required: value => !!value || "入力必須項目です。",
        email: value => {
          const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
          return pattern.test(value) || "メールアドレスを入力してください。";
        },
        password: value => {
          const pattern = /^[\w-]{6,24}$/;
          return (
            pattern.test(value) ||
            "パスワードは6文字以上、24文字以下で入力してください。"
          );
        },
        name: v =>
          (v && v.length <= 8) || "ニックネームは8文字以下で入力してください。"
      }
    };
  },
  middleware: ["checkAuth"],
  methods: {
    logIn() {
      this.$fireAuth
        .signInWithEmailAndPassword(this.login.email, this.login.password)
        .then(user => {
          alert("ログインに成功しました。");
          this.$router.push("/toppage/");
        })
        .catch(function(error) {
          const errorMessage = error.message;
          alert(errorMessage);
        });
    },
    testLogin(item) {
      this.login.email = item.email;
      this.login.password = item.password;
      this.logIn();
    },
    signUp() {
      this.$fireAuth
        .createUserWithEmailAndPassword(this.signup.email, this.signup.password)
        .then(async result => {
          const user = result.user;
          user.updateProfile({
            displayName: this.signup.nickname
          });
          try {
            const batch = this.$firestore.batch();
            // usersコレクションに登録ユーザーを追加
            batch.set(
              this.$firestore.collection("users").doc(user.uid),
              {
                name: this.signup.nickname,
                createdAt: this.$firebase.firestore.FieldValue.serverTimestamp()
              },
              { merge: true }
            );
            // profilesコレクションに登録ユーザーのプロフィールを追加
            batch.set(
              this.$firestore.collection("profiles").doc(user.uid),
              {
                id: user.uid,
                displayName: this.signup.nickname,
                createdAt: this.$firebase.firestore.FieldValue.serverTimestamp(),
                updatedAt: this.$firebase.firestore.FieldValue.serverTimestamp()
              },
              { merge: true }
            ),
              // 一括更新をコミット
              await batch.commit();
            alert("新規ユーザーとして登録されました。");
            this.$router.push("/toppage/");
          } catch (error) {
            await this.$fireAuth.user.delete();
            alert("エラーが発生しました。もう一度登録し直してください。");
          }
        })
        .catch(function(error) {
          const errorMessage = error.message;
          alert(errorMessage);
        });
    }
  }
};
</script>
