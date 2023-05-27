<script setup>
import { ref, onMounted, watch, nextTick } from "vue";

const props = defineProps({
  index: {
    type: Number,
    requied: true,
  },
  start: {
    type: Boolean,
    requied: true,
  },
  pause: {
    type: Boolean,
    required: true,
  },
});
const emits = defineEmits(["selected"]);
const flower = ref(null);
const animateEvent = ref(null);
const imageUrl = "../src/assets/" + (Math.floor(Math.random() * 10) + 1) + ".png";
const scale = Math.ceil(Math.random() * 10 + 1) / 10;
const left = Math.random() * document.body.getBoundingClientRect().width;
const styles = {
  top: "-200px",
  left:
    left < (Math.random() * document.body.getBoundingClientRect().width) / 2
      ? left - (left + Math.random() * 100) + "px"
      : left + "px",
  "background-size": "contain",
  "background-repeat": "no-repeat",
  "background-position": "center center",
  position: "fixed",
  transform: "rotate(0) scale(" + scale + ")",
  width: "auto",
  "z-index": "9998",
};

/**
 * Start animation
 */
const startAnimate = () => {
  if (flower.value) {
    animateEvent.value = flower.value.animate(
      [{ top: "110vh", transform: "rotate(360deg) scale(" + scale + ")" }],
      {
        duration: 10000,
        easing: "ease-out",
        fill: "both",
        iterations: 1,
      }
    );
  }
};

/**
 * Stop animation
 */
const stopAnimate = () => {
  animateEvent.value.pause();
  setTimeout(() => {
    flower.value.animate(
      [
        {
          top: "50%",
          left: "50%",
          transform: navigator.userAgent.match("Chrome")
            ? "translate(-50%, -50%) rotate(360deg) scale(0.1)"
            : "translate(-50%, -50%) rotate(360deg) scale(0)",
        },
      ],
      {
        duration: 1000,
        easing: "ease-out",
        fill: "both",
        iterations: 1,
      }
    );
  }, 1000);
};

/**
 * Flower click event
 */
const clickEvent = () => {
  emits("selected", props.index);
};

watch(
  () => props.pause,
  (newValue) => {
    if (newValue) {
      stopAnimate();
    }
  }
);

onMounted(() => {
  nextTick(() => {
    startAnimate();
  });
});
</script>

<template>
  <img ref="flower" :src="imageUrl" :style="styles" @click="clickEvent" />
</template>

<style scoped></style>
