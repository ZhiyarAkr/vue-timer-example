<template>
  <div class="flex flex-col">
    <div class="flex justify-center">
      <h1 class="text-4xl font-bold">
        <slot name="Header">
          <div class="text-gray-300">Stop Watch</div>
        </slot>
      </h1>
    </div>
    <Counter :count-up="true" :initial-count="0"></Counter>
  </div>
</template>
<script setup>
import Counter from "./Counter.vue";
</script>

<style scoped></style>
