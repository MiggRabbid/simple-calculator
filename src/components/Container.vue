<template>
  <div class="container" id="container">
    <input type="text" v-model="inputValue" @input="handleInput" class="input">
    <div class="buttons" @click="handleButton">
      <Button v-for="btn in buttons"
              :key="btn.value"
              :buttonValue="btn.value"
              :buttonType="btn.type"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import Button from "./ui/Button.vue";

console.log("----- Container");

export type typeInputValue = string;

export type typeButton = {
  value: string,
  type: "clear" | "delete" | "operator" | "number" | "result",
};

let inputValue = ref<typeInputValue>('');

const handleButton = (event: MouseEvent) => {
  const buttonValue = (event.target as HTMLElement).getAttribute('data-value');
  const buttonType = (event.target as HTMLElement).getAttribute('data-type');

  if (!buttonValue || !buttonType) return;

  switch (buttonType) {
    case 'clear':
      inputValue.value = '';
      return;
    case 'result':
      inputValue.value = evaluateExpression(inputValue.value);
      return;
    case 'delete':
      inputValue.value = inputValue.value.slice(0, -1);
      return;
    case 'operator':
      const lastChar = inputValue.value.slice(-1);
      if (lastChar === '.') {
        inputValue.value = inputValue.value.slice(0, -1) + buttonValue;
        return;
      }
      if ((lastChar === '*' && buttonValue === '/') || (lastChar === '/' && buttonValue === '*')) {
        inputValue.value = inputValue.value.slice(0, -1) + buttonValue;
        return;
      }
      inputValue.value += buttonValue;
      return;
    default:
      inputValue.value += buttonValue;
      return;
  }
}

const handleInput = (event: any) => {
  const currInput = event.target as HTMLInputElement;
  const sanitizedValue = currInput.value.replace(/[^\d+*/.-]/g, '');
  inputValue.value = sanitizedValue;
}

const evaluateExpression = (expression: string): string => {
  try {
    return eval(expression).toString();
  } catch (error) {
    console.error('Error evaluating expression:', error);
    return '';
  }
}

const buttons: typeButton[] = [
  { value: "C", type: "clear" },
  { value: "DEL", type: "delete" },
  { value: ".", type: "operator" },
  { value: "/", type: "operator" },

  { value: "7", type: "number" },
  { value: "8", type: "number" },
  { value: "9", type: "number" },
  { value: "*", type: "operator" },

  { value: "4", type: "number" },
  { value: "5", type: "number" },
  { value: "6", type: "number" },
  { value: "-", type: "operator" },

  { value: "1", type: "number" },
  { value: "2", type: "number" },
  { value: "3", type: "number" },
  { value: "+", type: "operator" },

  { value: "0", type: "number" },
  { value: "000", type: "number" },
  { value: "=", type: "result" },
];
</script>

<style scoped>
.container {
  @apply p-5 h-fit w-fit rounded-xl
  flex flex-col gap-8 items-center
  bg-lime-700 shadow-xl shadow-lime-900;
}

.input {
  @apply h-20 w-96 p-2 
  rounded-xl bg-amber-200 
  font-medium text-end text-3xl
  outline outline-1 outline-transparent ring-2 ring-transparent ring-opacity-0;
}

.input:focus{
  @apply -outline-offset-0 outline-amber-500 ring-amber-400 ring-opacity-75 border-0;
}

.input::-webkit-outer-spin-button,
.input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.buttons {
  @apply w-96 mx-auto justify-end flex flex-wrap gap-5;
}
</style>

