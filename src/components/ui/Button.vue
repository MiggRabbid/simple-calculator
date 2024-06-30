<template>
    <button type="button" :class="`button ${buttonType}`" key="buttonValue" :data-type="buttonType" :data-value="buttonValue">{{ buttonValue }}</button>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';

console.log("----- Button")

const props = defineProps<{
  buttonValue: string;
  buttonType: string;
}>();

const { buttonValue, buttonType } = props;
</script>

<style scoped>
.button {
  @apply h-20 w-20 rounded-3xl shadow-lg 
  font-medium text-2xl text-amber-400 text-opacity-75 align-middle
  transition-all duration-200;
}

.button:hover {
  @apply scale-105 text-opacity-100
}

.button:active {
  @apply scale-105 text-opacity-100 shadow-amber-400 shadow-md
}

.number {
  @apply bg-lime-900;
}

.clear, .delete, .operator {
  @apply bg-lime-800;
}

.result {
  @apply w-44 bg-amber-500 text-lime-800 text-4xl text-center
}
</style>