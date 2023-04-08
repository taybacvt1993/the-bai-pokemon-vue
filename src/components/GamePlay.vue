<template>
  <div class="screen">
    <h1>Pokemon</h1>
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((865 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 + 16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :img-back-face-url="`images/${card}.png`"
        :card="{ index, value: card }"
        @onFlip="checkRule($event)"
        ref="lstCardRef"
        :total="cardsContext.length"
      />
    </div>
    <h1>Pokemon</h1>
  </div>
</template>

<script setup>
import { ref } from "vue";
import CardFlip from "./Card.vue";

const emit = defineEmits(["onFinish"]);
const props = defineProps({
  cardsContext: {
    type: Array,
    default() {
      return [];
    },
  },
});

const lstCardRef = ref([]);
const rules = ref([]);
const checkRule = (card) => {
  if (rules.value.length === 2) return false;

  rules.value.push(card);
  if (rules.value.length === 2) {
    if (rules.value.every((e) => e.value == rules.value[0].value)) {
      rules.value = [];

      const disabledElements = document.querySelectorAll(".card.disabled");
      if (disabledElements?.length === props.cardsContext.length - 1) {
        setTimeout(() => {
          emit("onFinish");
        }, 2000);
      }
    } else {
      let idx1 = rules.value[0].index,
        idx2 = rules.value[1].index;
      setTimeout(() => {
        lstCardRef.value[idx1].onFlipBackCard();
        lstCardRef.value[idx2].onFlipBackCard();
      }, 1000);

      rules.value = [];
    }
  }
  return false;
};
</script>

<style scoped>
.screen {
  width: 100%;
  height: 100vh;
  z-index: 2;
  text-align: center;
  text-transform: uppercase;
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
