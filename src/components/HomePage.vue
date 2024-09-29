<script setup>
import { ref, onMounted } from "vue";

const canvas = ref(null);
const ctx = ref(null);
const array = ref([50, 20, 70, 10, 30, 90, 40, 60, 80, 100]);
const boxWidth = ref(0);
const sorting = ref(false);
const speed = ref(1);

const drawArray = () => {
  if (ctx.value) {
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
  }
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
        await new Promise((resolve) => setTimeout(resolve, 1000 / speed.value));
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

onMounted(() => {
  ctx.value = canvas.value.getContext("2d");
  boxWidth.value = canvas.value.width / array.value.length;
  drawArray();
});
</script>

<template>
  <div id="app" class="container">
    <canvas ref="canvas" width="600" height="400"></canvas>

    <div class="slidecontainer">
      <label for="speedRange" class="form-label">Speed</label>

      <input
        type="range"
        min="1"
        max="10"
        value="5"
        class="form-range"
        id="speedRange"
        v-model="speed"
      />
    </div>
    <p>{{ speed }}</p>
    <button
      type="button"
      class="btn btn-primary"
      @click="startSorting"
      :disabled="sorting"
    >
      Start Bubble Sort
    </button>
  </div>
</template>

<style scoped>
canvas {
  margin-bottom: 10px;
}
button {
  padding: 10px 20px;
  font-size: 16px;
}
</style>
