<script setup lang="ts">
import { onMounted } from "vue";
import { ref } from "vue";

const randomNumber = ref<number>(); 
const inputValues = ref<number[]>([]); //Get values from input to store here
const inputElement = ref<HTMLInputElement[]>([]); //HTMLInputElement mean dataType must be input
const timeRemaining = ref<number>(20);
let interval: number | undefined;
const validationMessage = ref<string>("");

const getInputElement = (el: HTMLInputElement) => {
  inputElement.value.push(el);
}
console.log(inputElement);


onMounted(() => {

  // Generate a random 6-digit number
  randomNumber.value = Math.floor(Math.random() * 900000 + 100000);

  // Start the timer
  interval = window.setInterval(() => {
    if (timeRemaining.value > 0) {
      timeRemaining.value--;
    } else {
      timeRemaining.value = 20;
      randomNumber.value = Math.floor(Math.random() * 900000 + 100000);
      inputValues.value = [];
      inputElement.value.forEach((el) => el.value = "");
      validationMessage.value = ""; // Clear validation message when the timer resets
      inputElement.value[0].focus();
    }
  }, 1000);

  // Attach event listeners to each input element
  inputElement.value.forEach((el: HTMLInputElement, index: number) => {
    const elementLength = inputElement.value.length;

    el.addEventListener('keyup', (event: KeyboardEvent) => {
      inputValues.value[index] = parseInt(el.value);

      if (event.key === "Backspace") {
        if (elementLength > 1) {
          const previousElement = el.previousElementSibling as HTMLInputElement;
          if (previousElement) {
            previousElement.focus();
          }
        }
        delete inputValues.value[index];
      } else if (!isNaN(parseInt(event.key))) {
        // console.log(event.key);
        if (el.value.length > 1) {
          el.focus();
          el.value = '';
        } else {
          const nextElement = el.nextElementSibling as HTMLInputElement;
          if (nextElement) {
            nextElement.focus();
          }
        }
        // console.log(el);
        
      } else {
        event.preventDefault();
      }

      // Check if all inputs are filled
      if (inputValues.value.length === elementLength) {
        const sixDigitsNumber = inputValues.value.join("");
        if (parseInt(sixDigitsNumber) === randomNumber.value) {
          validationMessage.value = 'Correct!';
          clearInterval(interval); // Stop the timer if the input is correct
        } else {
          validationMessage.value = "Wrong Number!";
        }
      }
    });
  });
})
</script>

<template>
  <div class="me h-screen flex flex-col justify-center gap-3">
    <h1 class="text-center text-2xl font-bold text-white">{{ randomNumber }}</h1>
    <div class="flex items-center justify-center gap-2">
      <input type="number" :ref="(el) => getInputElement(el as HTMLInputElement)" class="text-center w-[60px] h-10 border-2 rounded-md hide-arrow" 
        :class="{
          'border-green-500': validationMessage === 'Correct!',
          'border-red-500': validationMessage === 'Wrong Number!'
        }">
      <input type="number" :ref="(el) => getInputElement(el as HTMLInputElement)" class="text-center w-[60px] h-10 border-2 rounded-md hide-arrow" 
        :class="{
          'border-green-500': validationMessage === 'Correct!',
          'border-red-500': validationMessage === 'Wrong Number!'
        }">
      <input type="number" :ref="(el) => getInputElement(el as HTMLInputElement)" class="text-center w-[60px] h-10 border-2 rounded-md hide-arrow" 
        :class="{
          'border-green-500': validationMessage === 'Correct!',
          'border-red-500': validationMessage === 'Wrong Number!'

        }">
      <input type="number" :ref="(el) => getInputElement(el as HTMLInputElement)" class="text-center w-[60px] h-10 border-2 rounded-md hide-arrow" 
        :class="{
          'border-green-500': validationMessage === 'Correct!',
          'border-red-500': validationMessage === 'Wrong Number!'
        }">
      <input type="number" :ref="(el) => getInputElement(el as HTMLInputElement)" class="text-center w-[60px] h-10 border-2 rounded-md hide-arrow" 
        :class="{
          'border-green-500': validationMessage === 'Correct!',
          'border-red-500': validationMessage === 'Wrong Number!'
        }">
      <input type="number" :ref="(el) => getInputElement(el as HTMLInputElement)" class="text-center w-[60px] h-10 border-2 rounded-md hide-arrow" 
        :class="{
          'border-green-500': validationMessage === 'Correct!',
          'border-red-500': validationMessage === 'Wrong Number!'
        }">
    </div>
    <h1 class="text-center text-2xl font-bold text-red-500">{{ timeRemaining }}</h1>
    <p class="text-center text-2xl"
      :class="{
        'text-green-500': validationMessage === 'Correct!',
        'text-red-500': validationMessage === 'Wrong Number!'
      }">
      {{ validationMessage }}
    </p>
  </div>
</template>
<style scoped>
.me{
    background-color: rgb(171, 159, 159);
}
</style>
