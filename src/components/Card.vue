<template>
  <div
    class="card"
    @click="onFlipCard"
    :class="{ disabled: isFlipped }"
    :style="{
      height: `${(865 - 16 * 4) / Math.sqrt(total) - 16}px`,
      width: `${(((865 - 16 * 4) / Math.sqrt(total) - 16) * 3) / 4}px`,
      perspective: `${((((865 - 16 * 4) / Math.sqrt(total) - 16) * 3) / 4) * 2}px`,
    }"
  >
    <div class="card__inner" :class="{ 'is-flipped': isFlipped }">
      <div class="card__face card__face--front">
        <div class="card__content"></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{ backgroundImage: `url(${getImageUrl(imgBackFaceUrl)})` }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
const emit = defineEmits(["onFlip"]);
const prop = defineProps({
  imgBackFaceUrl: {
    type: String,
    required: true,
  },
  card: {
    type: [Number, Object],
    required: true,
  },
  total: Number,
});
defineExpose({
  onFlipBackCard,
});

const getImageUrl = (name) => {
  return new URL("../assets/" + name, import.meta.url).href;
};

const isFlipped = ref(false);
function onFlipCard() {
  if (!isFlipped.value) {
    isFlipped.value = !isFlipped.value;
    // setTimeout(() => (isFlipped.value = !isFlipped.value), 1000);
    emit("onFlip", prop.card);
  }
}

function onFlipBackCard() {
  isFlipped.value = false;
}
</script>

<style scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
  /* width: 90px;
  height: 120px; */
}

.card.disabled .card__inner {
  cursor: default;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  width: 100%;
  height: 100%;
  position: absolute;
  backface-visibility: hidden;
  /* overflow: hidden; */
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
  border-radius: 1rem;
  padding: 1rem;
}

.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face--back .card__content {
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  height: 100%;
  width: 100%;
  transform: scale(1.2);
}

.card__face--front {
  background-color: var(--dark);
}
.card__face--front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  background-size: contain;
  height: 100%;
  width: 100%;
}
</style>
