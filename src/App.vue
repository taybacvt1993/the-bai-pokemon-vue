<script setup>
import { reactive, ref } from "vue";
import StartScreen from "./components/StartScreen.vue";
import GamePlay from "./components/GamePlay.vue";
import ResultScreen from "./components/ResultScreen.vue";

const status = {
  start: 0,
  game: 1,
  end: 2,
};
const isStarted = ref(status.start);
const settings = reactive({
  total: 0,
  cardsContext: [],
  startedAt: null,
  time: null,
});

function onHandleBeforeStart(e) {
  const cards_sample_1 = Array.from({ length: e / 2 }, (_, x) => x + 1);
  const cards_sample_2 = [...cards_sample_1];
  const card = [...cards_sample_1, ...cards_sample_2].sort((a, b) => 0.5 - Math.random());
  console.log(card);
  settings.cardsContext = card;
  settings.total = e;
  settings.startedAt = new Date().getTime();
  // data ready
  isStarted.value = status.game;
}

function onGetResult() {
  console.log("You win");
  settings.time = ((new Date().getTime() - settings.startedAt) / 1000 - 2).toFixed(2); // timeout finish 2s
  isStarted.value = status.end;
}
</script>

<template>
  <start-screen v-if="isStarted === status.start" @onStart="onHandleBeforeStart($event)" />
  <game-play
    v-if="isStarted === status.game"
    :cardsContext="settings.cardsContext"
    @on-finish="onGetResult"
  />
  <result-screen
    v-if="isStarted === status.end"
    :time="settings.time"
    @on-start-again="isStarted = status.start"
  />
</template>

<style scoped></style>
