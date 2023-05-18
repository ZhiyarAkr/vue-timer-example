<template>
  <div class="flex items-center justify-center">
    <div class="text-center">
      <h1 class="text-4xl mb-4 text-gray-300 font-bold">Timer</h1>
      <div class="mb-4">
        <label for="initialCount" class="mr-2 text-gray-300"
          >Start up Time(seconds):</label
        >
        <input
          id="initialCount"
          type="number"
          v-model="userInitialCount"
          class="border px-2 py-1"
          min="1"
          max="50"
        />
      </div>
      <Counter
        :count-up="false"
        :initial-count="timerInitialCount"
        @on-timer-count-up="handleOnTimerUp"
      >
        <template #startButton>
          <div
            class="px-4 py-2 bg-lime-500 hover:opacity-80 text-white rounded"
          >
            Custom Start
          </div>
        </template>
      </Counter>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import Counter from "./Counter.vue";

const userInitialCount = ref(0);
const timerInitialCount = computed(() => {
  return userInitialCount.value * 1000;
});
const emit = defineEmits(["onTimerCountUp"]);

const handleOnTimerUp = () => {
  emit("onTimerCountUp");
};
</script>

<style scoped></style>
