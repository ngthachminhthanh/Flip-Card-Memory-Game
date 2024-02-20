<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        // set dynamic width height cho khung chứa các thẻ bài
        //width: `${
        //((((vh - 16 * 4) / Math.sqrt(cardsAfterMixed.length) - 16) * 3) / 4 +
        //  16) *
        //Math.sqrt(cardsAfterMixed.length)
        //}px`,
        width: `${
          (vw / Math.sqrt(cardsAfterMixed.length) - 16 + 16) *
          Math.sqrt(cardsAfterMixed.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsAfterMixed"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :cardsAfterMixed="cardsAfterMixed"
        :card="{ index: index, value: card }"
        @onFlip="checkRule($event)"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./Card.vue";
export default {
  props: {
    cardsAfterMixed: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      cardsHandle: [],
      vh: Math.max(
        document.documentElement.clientHeight || 0,
        window.innerHeight || 0
      ),
      vw: Math.max(
        document.documentElement.clientWidth || 0,
        window.innerWidth || 0
      ),
    };
  },
  methods: {
    checkRule(card) {
      if (this.cardsHandle.length === 2) return false;

      this.cardsHandle.push(card);

      // trường hợp 2 thẻ được lật giống nhau (khác vị trí)
      if (
        this.cardsHandle.length === 2 &&
        this.cardsHandle[0].value === this.cardsHandle[1].value &&
        this.cardsHandle[0].index !== this.cardsHandle[1].index
      ) {
        // thêm class 'disabled' vào component card để không cho lật nữa
        this.$refs[
          `card-${this.cardsHandle[0].index}`
        ][0].onEnableDisableMode();
        this.$refs[
          `card-${this.cardsHandle[1].index}`
        ][0].onEnableDisableMode();
        // reset cardsHandle
        this.cardsHandle = [];
        // check win: số lượng card disabled = số lượng cards render
        const disabledElement = document.querySelectorAll(
          ".screen .card.disabled"
        );
        if (
          disabledElement &&
          disabledElement.length === this.cardsAfterMixed.length - 2
        ) {
          // tạo độ trễ để player thấy được 2 cards cuối cùng được lật trước khi chuyển sang ResultScreen
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (
        this.cardsHandle.length === 2 &&
        this.cardsHandle[0].index === this.cardsHandle[1].index
      ) {
        setTimeout(() => {
          // close 2 cards
          this.$refs[`card-${this.cardsHandle[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.cardsHandle[1].index}`][0].onFlipBackCard();
          // reset cardsHandle
          this.cardsHandle = [];
        }, 800);
      }
      // trường hợp 2 thẻ được lật khác nhau
      else if (
        this.cardsHandle.length === 2 &&
        this.cardsHandle[0].value !== this.cardsHandle[1].value
      ) {
        setTimeout(() => {
          // close 2 cards
          this.$refs[`card-${this.cardsHandle[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.cardsHandle[1].index}`][0].onFlipBackCard();
          // reset cardsHandle
          this.cardsHandle = [];
        }, 800);
      } else return false;
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: auto;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
