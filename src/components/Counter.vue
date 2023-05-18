<template>
  <div class="flex flex-col items-center">
    <div class="text-4xl font-bold text-gray-300">
      {{ countUp ? formatTimeSW : formatTimeTimer }}
    </div>
    <div class="mt-4 space-x-2">
      <button @click="startTimer" :disabled="isTimerRunning">
        <slot name="startButton">
          <div
            class="px-4 py-2 bg-blue-500 hover:bg-blue-600 text-white rounded"
          >
            Start
          </div>
        </slot>
      </button>

      <button
        @click="stopTimer"
        :disabled="!isTimerRunning"
        class="px-4 py-2 bg-red-500 hover:bg-red-600 text-white rounded"
      >
        Stop
      </button>
      <button
        @click="resetTimer"
        class="px-4 py-2 bg-gray-500 hover:bg-gray-600 text-white rounded"
      >
        Reset
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, onBeforeUnmount } from "vue";

const props = defineProps({
  countUp: Boolean,
  initialCount: Number,
});

const emit = defineEmits(["onTimerCountUp"]);

const startTime = ref(0);
const elapsedTime = ref(0);
const timerInterval = ref(null);

const isTimerRunning = computed(() => {
  return timerInterval.value !== null;
});

const computedRemainingTime = computed(() => {
  if (props.initialCount - elapsedTime.value > 0) {
    return props.initialCount - elapsedTime.value;
  } else {
    return 0;
  }
});

const formatTimeTimer = computed(() => {
  const minutes = Math.floor(computedRemainingTime.value / 60000);
  const seconds = Math.floor((computedRemainingTime.value % 60000) / 1000);
  const milliseconds = Math.floor((computedRemainingTime.value % 1000) / 10);

  return `${padNumber(minutes)}:${padNumber(seconds)}:${padNumber(
    milliseconds,
    2
  )}`;
});

const formatTimeSW = computed(() => {
  const minutes = Math.floor(elapsedTime.value / 60000);
  const seconds = Math.floor((elapsedTime.value % 60000) / 1000);
  const milliseconds = Math.floor((elapsedTime.value % 1000) / 10);

  return `${padNumber(minutes)}:${padNumber(seconds)}:${padNumber(
    milliseconds,
    2
  )}`;
});

const startTimer = () => {
  if (!props.countUp && computedRemainingTime.value <= 0) {
    return;
  }
  if (props.initialCount == null) {
    return;
  }
  if (!isTimerRunning.value) {
    startTime.value = Date.now() - elapsedTime.value;
    timerInterval.value = setInterval(() => {
      elapsedTime.value = Date.now() - startTime.value;
    }, 10);
  }
};

const stopTimer = () => {
  if (isTimerRunning.value) {
    clearInterval(timerInterval.value);
    timerInterval.value = null;
  }
};

const resetTimer = () => {
  startTime.value = 0;
  elapsedTime.value = 0;
  stopTimer();
};

const padNumber = (number, width = 2) => {
  return String(number).padStart(width, "0");
};

onBeforeUnmount(() => {
  stopTimer();
});

watch(computedRemainingTime, (newVal) => {
  if (newVal === 0) {
    stopTimer();
    emit("onTimerCountUp");
  }
});
</script>

<style scoped></style>
