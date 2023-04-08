<template>
  <h1>✨ Congratulations ✨</h1>
  <p>{{ time }} seconds</p>
  <button @click="$emit('onStartAgain')">Start Again</button>
</template>

<script setup>
defineProps({
  time: {
    type: Number,
    required: true,
  },
});
</script>
