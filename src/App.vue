
<template>
  <div class="calculator">
    <div class="screen">{{ display }}</div>
    <div class="buttons">
      <button @click="appendNumber('1')">1</button>
      <button @click="appendNumber('2')">2</button>
      <button @click="appendNumber('3')">3</button>
      <button @click="appendNumber('4')">4</button>
      <button @click="appendNumber('5')">5</button>
      <button @click="appendNumber('6')">6</button>
      <button @click="appendNumber('7')">7</button>
      <button @click="appendNumber('8')">8</button>
      <button @click="appendNumber('9')">9</button>
      <button @click="appendNumber('0')">0</button>

      <button @click="chooseOperation('+')">+</button>
      <button @click="chooseOperation('-')">-</button>
      <button @click="chooseOperation('*')">*</button>
      <button @click="chooseOperation('/')">/</button>

      <button @click="clear">C</button>
      <button @click="calculate">=</button>
    </div>
  </div>
</template>

<script lang="ts">


  import { ref } from 'vue';

  export default {
    setup() {
      const display = ref('');
      const currentOperation = ref('');
      const firstOperand = ref('');
      const secondOperand = ref('');

      const appendNumber = (num: string) => {
        if (currentOperation.value) {
          secondOperand.value += num;
          display.value = secondOperand.value;
        } else {
          firstOperand.value += num;
          display.value = firstOperand.value;
        }
      };

      const chooseOperation = (operation: string) => {
        if (!firstOperand.value) return;
        currentOperation.value = operation;
      };

      const calculate = () => {
        if (!firstOperand.value || !secondOperand.value || !currentOperation.value) return;

        const num1 = parseFloat(firstOperand.value);
        const num2 = parseFloat(secondOperand.value);
        let result = 0;

        switch (currentOperation.value) {
          case '+':
            result = num1 + num2;
            break;
          case '-':
            result = num1 - num2;
            break;
          case '*':
            result = num1 * num2;
            break;
          case '/':
            result = num1 / num2;
            break;
        }

        display.value = result.toString();
        firstOperand.value = result.toString();
        secondOperand.value = '';
        currentOperation.value = '';
      };

      const clear = () => {
        display.value = '';
        firstOperand.value = '';
        secondOperand.value = '';
        currentOperation.value = '';
      };

      return {display, appendNumber, chooseOperation, calculate, clear};
    }
  };
</script>

<style>
.calculator {
  max-width: 300px;
  margin: 0 auto;
  text-align: center;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  background: #f9f9f9;
}

.screen {
  font-size: 24px;
  padding: 8px;
  margin-bottom: 16px;
  background: #eee;
  border: 1px solid #ccc;
  border-radius: 4px;
  min-height: 40px;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 8px;
}

button {
  font-size: 18px;
  padding: 8px;
  border: none;
  border-radius: 4px;
  background: #007bff;
  color: white;
  cursor: pointer;
}

button:hover {
  background: #0056b3;
}

button:active {
  background: #003f7f;
}
</style>
