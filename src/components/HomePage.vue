<script setup>
import { ref, onMounted, watch } from "vue";

const canvas = ref(null);
const ctx = ref(null);
const array = ref([]);
const boxWidth = ref(0);
const sorting = ref(false);
const speed = ref(5);
const size = ref(5);

function generateRandomNumbers(count, min, max) {
  return Array.from(
    { length: count },
    () => Math.floor(Math.random() * (max - min + 1)) + min
  );
}

const createArray = () => {
  array.value = generateRandomNumbers(size.value * 5, 10, 100);
  boxWidth.value = canvas.value.width / array.value.length;
  drawArray();
};

const drawArray = () => {
  if (!ctx.value) return;
  ctx.value.clearRect(0, 0, canvas.value.width, canvas.value.height);
  array.value.forEach((value, i) => {
    ctx.value.fillStyle = "blue";
    ctx.value.fillRect(
      i * boxWidth.value,
      canvas.value.height - value,
      boxWidth.value - 2,
      value
    );
  });
};

const bubbleSort = async () => {
  sorting.value = true;
  const len = array.value.length;
  for (let i = 0; i < len - 1; i++) {
    for (let j = 0; j < len - i - 1; j++) {
      if (array.value[j] > array.value[j + 1]) {
        const temp = array.value[j];
        array.value[j] = array.value[j + 1];
        array.value[j + 1] = temp;
        drawArray();
        await new Promise((resolve) => setTimeout(resolve, 200 / speed.value));
      }
    }
  }
  sorting.value = false;
};

const startSorting = () => {
  if (!sorting.value) {
    bubbleSort();
  }
};

watch(size, createArray);
watch(speed, drawArray);

onMounted(() => {
  ctx.value = canvas.value.getContext("2d");
  createArray();
});
</script>

<template>
  <div id="app" class="container my-4">
    <h1 class="text-center mb-4">Sorting Algorithm Visualization</h1>

    <div class="row">
      <div class="col-12">
        <canvas ref="canvas" class="w-100" width="800" height="200"></canvas>
      </div>
    </div>

    <div class="mt-4 row align-items-center">
      <div class="col-2 text-end">
        <span class="fs-3">Speed</span>
      </div>
      <div class="col-8">
        <input
          type="range"
          min="1"
          max="10"
          v-model="speed"
          class="form-range"
          id="speedRange"
        />
      </div>
      <div class="col-2">
        <span class="fs-3">{{ speed }}</span>
      </div>
    </div>

    <div class="mt-4 row align-items-center">
      <div class="col-2 text-end">
        <span class="fs-3">Size</span>
      </div>
      <div class="col-8">
        <input
          type="range"
          min="1"
          max="10"
          v-model="size"
          class="form-range"
          id="sizeRange"
        />
      </div>
      <div class="col-2">
        <span class="fs-3">{{ size }}</span>
      </div>
    </div>

    <div class="text-center mt-4">
      <button
        type="button"
        class="btn btn-primary"
        @click="startSorting"
        :disabled="sorting"
      >
        Start
      </button>
    </div>
  </div>
</template>
