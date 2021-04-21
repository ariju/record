<template>
  <v-main>
    <v-item-group mandatory>
      <v-sheet class="display-2 pt-10"> BMI </v-sheet>
      <v-form class="pa-5">
        <v-text-field
        v-model="weight" 
        outlined 
        type="text" 
        placeholder="体重kg"
        style="width:150px"
        ></v-text-field>
        <v-text-field 
        v-model="height"
        outlined
        type="text" 
        placeholder="身長cm" 
        style="width:150px"
        ></v-text-field>
      </v-form>
      <v-btn @click="calculate" color="primary">
        <v-icon left small>mdi-calculator</v-icon>
        計算する</v-btn
      >
      <v-btn href="#" @click.prevent="reset()" color="error">
        <v-icon left small>mdi-restart</v-icon>
        Reset
      </v-btn>
      <div v-if="result" class="ma-5 headline">
        BMI結果 : {{ Math.floor(result * 10) / 10 }}
        <div>
          <div>標準体重 : {{ Math.floor(standard * 10) / 10 }}</div>
          <div class="pr-13">
            適正体重まで :
            {{ Math.floor(Appropriate * 10) / 10 }}
          </div>
          <div class="pr-5">肥満度判定 : {{ Judgment }}</div>
        </div>
      </div>
      <div v-if="error">{{ error }}</div>
      <v-row class="justify-center">
        <v-list>
          <v-list-item>BMI指数 = 体重 / (身長 * 身長)</v-list-item>
          <v-list-item>17未満 痩せすぎ</v-list-item>
          <v-list-item>17以上〜20未満 痩せ気味</v-list-item>
          <v-list-item>20以上〜24未満 普通</v-list-item>
          <v-list-item>24以上〜30未満 肥満</v-list-item>
          <v-list-item>30以上〜35未満 肥満2</v-list-item>
          <v-list-item>35以上〜40未満 肥満3</v-list-item>
          <v-list-item>40以上 肥満4</v-list-item>
        </v-list>
      </v-row>

      <p>
        BMIとは体重と身長のバランスをチェックし、 外見的な肥満度を測定するもの
        <br />
        BMI指数 = 体重(kg) ÷ {身長(m) x 身長(m)}
      </p>
    </v-item-group>
  </v-main>
</template>

<script>
export default {
  name: "bmi",
  data() {
    return {
      drawer: null,
      weight: "",
      height: "",
      result: "",
      error: "",
      Judgment: "",
      standard: "",
      Appropriate: "",
    };
  },
  methods: {
    calculate() {
      if (this.weight && this.height) {
        this.height = this.height / 100;
        this.result = this.weight / (this.height * this.height);
        this.error = "";
      } else {
        this.error = "体重と身長を入力してください！";
      }
      this.standard = this.height * this.height * 22;
      this.Appropriate = this.standard - this.weight;
      if (this.result <= "17") {
        this.Judgment = "痩せすぎ";
      } else if (this.result <= "20") {
        this.Judgment = "痩せ気味";
      } else if (this.result <= "24") {
        this.Judgment = "普通";
      } else if (this.result <= "30") {
        this.Judgment = "肥満";
      } else if (this.result <= "35") {
        this.Judgment = "肥満2";
      } else if (this.result <= "40") {
        this.Judgment = "肥満3";
      } else if (this.result >= "40") {
        this.Judgment = "肥満4";
      }
    },
    reset() {
      this.height = "";
      this.weight = "";
      this.result = "";
      this.error = "";
    },
  },
};
</script>
