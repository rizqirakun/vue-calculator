<script setup lang="ts"></script>

<template>
  <div className="calculator">
    <div className="calculator-display">
      <span
        className="auto-scaling-text"
        :style="{
          fontSize:
            currentValue.length > 8
              ? '.5em'
              : currentValue.length > 5
              ? '.75em'
              : '',
        }"
        >{{ currentValue }}</span
      >
      <span className="operator-display">{{ displayOperator }}</span>
    </div>
    <div className="calculator-keypad">
      <div className="input-keys">
        <div className="function-keys">
          <button
            type="button"
            className="calculator-key key-clear"
            @click="toggleClear()"
          >
            {{ clearText }}
          </button>
          <button
            type="button"
            className="calculator-key key-sign"
            @click="toggleSign()"
          >
            ±
          </button>
          <button
            type="button"
            className="calculator-key key-percent"
            @click="togglePercent()"
          >
            %
          </button>
        </div>
        <div className="digit-keys">
          <button
            type="button"
            className="calculator-key key-0"
            @click="inputDigit(0)"
          >
            0
          </button>
          <button
            type="button"
            className="calculator-key key-dot"
            @click="inputDecimal()"
          >
            ●
          </button>
          <button
            type="button"
            className="calculator-key key-1"
            @click="inputDigit(1)"
          >
            1
          </button>
          <button
            type="button"
            className="calculator-key key-2"
            @click="inputDigit(2)"
          >
            2
          </button>
          <button
            type="button"
            className="calculator-key key-3"
            @click="inputDigit(3)"
          >
            3
          </button>
          <button
            type="button"
            className="calculator-key key-4"
            @click="inputDigit(4)"
          >
            4
          </button>
          <button
            type="button"
            className="calculator-key key-5"
            @click="inputDigit(5)"
          >
            5
          </button>
          <button
            type="button"
            className="calculator-key key-6"
            @click="inputDigit(6)"
          >
            6
          </button>
          <button
            type="button"
            className="calculator-key key-7"
            @click="inputDigit(7)"
          >
            7
          </button>
          <button
            type="button"
            className="calculator-key key-8"
            @click="inputDigit(8)"
          >
            8
          </button>
          <button
            type="button"
            className="calculator-key key-9"
            @click="inputDigit(9)"
          >
            9
          </button>
        </div>
      </div>
      <div className="operator-keys">
        <button
          type="button"
          className="calculator-key key-divide"
          @click="setOperator('/')"
        >
          ÷
        </button>
        <button
          type="button"
          className="calculator-key key-multiply"
          @click="setOperator('*')"
        >
          ×
        </button>
        <button
          type="button"
          className="calculator-key key-subtract"
          @click="setOperator('-')"
        >
          −
        </button>
        <button
          type="button"
          className="calculator-key key-add"
          @click="setOperator('+')"
        >
          +
        </button>
        <button
          type="button"
          className="calculator-key key-equals"
          @click="setOperator('=')"
        >
          =
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: "Calculator",
  data() {
    return {
      previousValue: "0",
      currentValue: "0",
      clearText: "C",
      displayOperator: "",
      reset: false,
    };
  },
  methods: {
    inputDigit(number: number) {
      this.currentValue =
        this.currentValue === "0" || this.reset
          ? number.toString()
          : this.currentValue + number.toString();

      if (this.reset) {
        this.reset = !this.reset;
      }
    },
    inputDecimal() {
      if (!this.currentValue.includes(".")) {
        this.currentValue = this.currentValue + ".";
      }
    },
    toggleClear() {
      this.currentValue = "0";
      this.displayOperator = "";
      this.previousValue = "0";
    },
    toggleSign() {
      if (this.currentValue === "0") return;

      this.currentValue = (parseFloat(this.currentValue) * -1).toString();
    },
    togglePercent() {
      if (this.currentValue === "0") return;
      this.currentValue = (parseFloat(this.currentValue) / 100).toString();
    },
    setOperator(symbol: string) {
      if (!this.displayOperator && symbol !== "=") {
        this.displayOperator = symbol;
        this.previousValue = this.currentValue;
        this.reset = true;
        return;
      }

      if (this.displayOperator && this.reset) {
        this.displayOperator = symbol;
        return;
      }

      let value = parseFloat(this.previousValue);
      let current = parseFloat(this.currentValue);

      switch (this.displayOperator) {
        case "+":
          value += current;
          break;
        case "-":
          value -= current;
          break;
        case "*":
          value *= current;
          break;
        case "/":
          value /= parseFloat(
            current.toFixed(Math.min(this.currentValue.length, 8))
          );
          break;
        default:
          break;
      }

      this.currentValue = value.toString();

      if (symbol === "=") {
        this.displayOperator = "";
        this.previousValue = "0";
        return;
      }

      this.displayOperator = symbol;
      this.previousValue = this.currentValue;
      this.reset = true;
    },
  },
};
</script>
