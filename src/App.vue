<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'in-game'"
    :cardsAfterMixed="settings.cardsAfterMixed"
    @onFinish="onGetResult"
  />
  <result-screen
    :timer="timer"
    v-if="statusMatch === 'result'"
    @onStartAgain="statusMatch = 'default'"
  />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";

import { shuffled } from "./utils/array";

export default {
  name: "App",
  data() {
    return {
      settings: {
        quantity: 0,
        cardsAfterMixed: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.quantity = config.quantity;

      // Ý tưởng là tạo ra 2 mảng, mỗi mảng chứa giá trị từ 1 -> quantity/2
      // VD: 4x4 = 16 cards, thì cần 2 mảng [1,2,...,8] => tạo ra 8 cặp giống nhau
      //     Trộn 2 mảng lại và random vị trí
      const firstCards = Array.from(
        { length: this.settings.quantity / 2 },
        (_, i) => i + 1
      );

      const secondCards = [...firstCards];
      const combineCards = [...firstCards, ...secondCards];
      this.settings.cardsAfterMixed = shuffled(shuffled(combineCards)); // trộn 2 lần

      // bắt đầu tính giờ và chuyển sang "in-game"
      // thời gian chơi = endedAt - startedAt
      this.settings.startedAt = new Date().getTime();

      // data ready
      this.statusMatch = "in-game";
    },
    onGetResult() {
      // tính thời gian chơi
      this.timer = new Date().getTime() - this.settings.startedAt;
      // chuyển sang result component
      this.statusMatch = "result";
    },
  },
};
</script>
