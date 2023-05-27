<script setup>
import Flower from "./components/Flower.vue";
import { ref } from "vue/dist/vue.esm-bundler";

const timer = ref(null);
const started = ref(false);
const flowerList = ref([]);
const selectFlower = ref(null);

/**
 * Game start
 */
const play = () => {
  started.value = true;
  let cnt = 0;
  window.clearInterval(timer.value);
  timer.value = setInterval(function () {
    flowerList.value.push({
      index: cnt,
      start: true,
      pause: false,
    });
    setTimeout(() => {
      flowerList.value.shift();
    }, 10000);
    cnt++;
  }, 500);
};

/**
 * Selected one flower
 * 
 * @param {number} index 
 */
const selectedOne = async (index) => {
  if (selectFlower.value !== null) {
    return;
  }

  let flowerIndex = flowerList.value.findIndex((element) => element.index == index);
  if (flowerList.value[flowerIndex]) {
    flowerList.value[flowerIndex].pause = true;
    selectFlower.value = flowerIndex;
    clearInterval(timer.value);
  }
};
</script>

<template>
  <div class="content">
    <div class="main">
      <div class="text-center">
        <img class="title" src="./assets/title.png" />
      </div>
      <div class="position-relative">
        <p
          v-if="!started"
          class="position-absolute w-100"
          style="max-width: 900px; bottom: 5%"
        >
          <img src="./assets/start.png" class="start-btn start-btn-blink" @click="play" />
        </p>
        <img src="./assets/letter.png" class="letter" />
      </div>
      <div class="position-relative text-right">
        <img id="niu1" src="./assets/niuniu.png" class="niuniu" />
      </div>
    </div>
  </div>
  <div id="niu2" style="display: none">
    <img src="./assets/niuniu2.png" class="bottom-niu" />
  </div>
  <Flower
    v-for="v in flowerList"
    :key="v.index"
    :index="v.index"
    :start="v.start"
    :pause="v.pause"
    @selected="selectedOne"
  />
</template>

<style scoped>
body {
  margin: 0;
  padding: 0;
  border: none;
}
.content {
  background-color: #f9d2d6;
  padding: 0rem;
  width: 100vw;
  height: 100vh;
}
.content img {
  width: 100%;
}
.content .main {
  width: 80%;
  max-width: 600px;
  margin: 0 auto;
}
.text-center {
  text-align: center;
}
.start-btn {
  width: 50% !important;
  opacity: 0;
}
.start-btn-blink {
  animation: blink 3s infinite;
}
@keyframes blink {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.1;
  }
}
.niuniu {
  width: 35% !important;
  margin-top: -35%;
}
.position-relative {
  position: relative;
}
.position-absolute {
  position: absolute;
}
.w-100 {
  width: 100%;
}
.text-right {
  text-align: right;
}
</style>
