<template>
  <v-app>
    <div class="content">
      <div class="content_title">
        <div class="TextAnime1">
          <span
            v-for="(t, index) in text"
            :key="index"
            class="item"
            :style="{ animationDelay: index * 100 + 'ms' }"
            v-text="t"
          />
        </div>
        <nuxt />
      </div>
    </div>
    <!-- メイン２ -->
    <v-card height="380" class="main">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          <div class="post-preview">
            <h1 class="post-title text-center ma-8 display-1 font-weight-bold">
              アプリ紹介
            </h1>
            <hr />
            <h2 class="post-subtitle ma-5 headline font-weight-bold">
              筋力トレーニング管理アプリ
            </h2>
            <p class="post-meta font-weight-bold mx-6 title">
              このアプリはカレンダーを使用して、その日、自分がやったトレーニングを、
              記録できるアプリです。記録機能の他に自分の体重、身長を入力して、
              BMI指数を図る機能とトレーニングの時に使用出来るカウントダウンタイマーが
              ついています。
            </p>
          </div>
        </div>
      </div>
    </v-card>
    <h2 class="text-center ma-6">アプリで出来ること</h2>
    <h2 class="text-center" style="color: gray">bmi指数を図る</h2>
    <v-card class="text-center">
      <video
        :src="require('@/assets/videos/bmivideos.mp4')"
        loop
        autoplay
        muted
      ></video>
    </v-card>
    <h2 class="text-center ma-10" style="color: gray">calendarに記録をとる</h2>
    <v-card class="text-center">
      <video
        :src="require('@/assets/videos/calendar.mp4')"
        loop
        autoplay
        muted
        width="100%"
      ></video>
    </v-card>
    <h2 class="text-center ma-10 color" style="color: gray">
      カウントダウンタイマー
    </h2>
    <v-card class="text-center">
      <video
        :src="require('@/assets/videos/countdowntimer.mp4')"
        loop
        autoplay
        muted
        width="100%"
      ></video>
    </v-card>
    <!-- フッター -->
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
  data() {
    return {
      drawer: null,
      text: "トレーニングお疲れ様でした。今日の成果を記録しよう！"
    };
  },
  methods: {
    logOut() {
      if (window.confirm("ログアウトしますか？")) {
        this.$fireAuth.signOut().then(() => {
          alert("ログアウトしました。");
          this.$router.push("/");
        });
      }
    }
  }
};
</script>

<style scoped>
.content {
  height: 720px;
  display: flex;
  color: white;
  width: 100%;
  background-size: cover;
  background-image: url("~@/assets/images/danbel.jpg");
  position: relative;
}
.content_title {
  color: white;
  margin: auto;
  height: 177px;
  width: 643px;
  padding-left: 48px;
  font-weight: bold;
  font-family: serif;
  letter-spacing: 2px;
}
@keyframes text-in {
  0% {
    transform: translate(0, -20px);
    opacity: 0;
  }
}
.item {
  display: inline-block;
  min-width: 0.3em;
  font-size: 3rem;
  animation: text-in 0.8s cubic-bezier(0.22, 0.15, 0.25, 1.43) 0s backwards;
}
.fot {
  font-size: 30px;
}
.main {
  width: 100%;
  background-size: cover;
  background-color: #dddddd;
}
</style>
