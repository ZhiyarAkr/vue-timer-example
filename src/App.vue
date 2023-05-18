<template>
  <div class="flex flex-col h-screen justify-around">
    <Timer @onTimerCountUp="handleOnTimerUp"></Timer>
    <StopWatch>
      <template #Header>
        <div class="bg-lime-300 text-gray-900">Stop Watch (custom)</div>
      </template>
    </StopWatch>
  </div>
</template>

<script setup>
import Timer from "./components/Timer.vue";
import StopWatch from "./components/StopWatch.vue";

const handleOnTimerUp = () => {
  alert("Timer is Up!!");
};
</script>

<style scoped></style>
