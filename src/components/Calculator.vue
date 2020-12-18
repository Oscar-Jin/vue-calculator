<template>
  <div class="container">
    <div class="display">{{ current || "0" }}</div>
    <button @click="clear" class="utility">{{ previous ? "C" : "AC" }}</button>
    <button @click="sign" class="utility">+/-</button>
    <button @click="percent" class="utility">%</button>
    <button @click="divide" class="operator">÷</button>
    <button @click="append('7')" class="numpad">7</button>
    <button @click="append('8')" class="numpad">8</button>
    <button @click="append('9')" class="numpad">9</button>
    <button @click="times" class="operator">x</button>
    <button @click="append('4')" class="numpad">4</button>
    <button @click="append('5')" class="numpad">5</button>
    <button @click="append('6')" class="numpad">6</button>
    <button @click="minus" class="operator">-</button>
    <button @click="append('1')" class="numpad">1</button>
    <button @click="append('2')" class="numpad">2</button>
    <button @click="append('3')" class="numpad">3</button>
    <button @click="add" class=" operator">+</button>
    <button @click="append('0')" class="numpad zero">0</button>
    <button @click="dot" class="numpad">.</button>
    <button @click="equal" class="operator">=</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      previous: "",
      current: "",
      operator: null,
      operatorClicked: false,
      recursiveMode: false,
    };
  },
  methods: {
    clear() {
      if (this.current && this.previous) {
        this.previous = this.current;
        this.current = "";
      } else {
        this.current = "";
        this.previous = "";
      }
    },
    sign() {
      this.current =
        this.current.charAt(0) === "-"
          ? this.current.slice(1)
          : `-${this.current}`;
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = "";
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    },
    dot() {
      if (this.current.indexOf(".") === -1) {
        this.append(".");
      }
    },
    setPrevious() {
      if (this.current) {
        this.previous = this.current;
      }
      this.operatorClicked = true;
      this.recursiveMode = false;
    },
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    times() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    minus() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    add() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    equal() {
      if (this.recursiveMode) {
        const result = `${this.operator(
          parseFloat(this.current || 0),
          parseFloat(this.previous || 0)
        )}`;
        this.current = result;
      } else {
        const result = `${this.operator(
          parseFloat(this.previous || 0),
          parseFloat(this.current || 0)
        )}`;
        this.previous = this.current;
        this.current = result;
        this.recursiveMode = true;
      }
    },
  },
};
</script>

<style scoped>
.container {
  @apply grid grid-cols-4 row-auto text-3xl text-white rounded-2xl overflow-hidden shadow;
  border: 1px solid #545256;
  background-color: #545256;
  grid-gap: 1px;
}
.display {
  @apply col-span-4 pt-6 px-6 text-6xl font-thin text-right;
  background-color: #545256;
}
.utility {
  @apply text-center py-4 outline-none;
  background-color: #656367;
}
.utility:active {
  outline: none;
  background-color: #7e7d80;
}
.numpad {
  @apply text-center py-4 outline-none;
  background-color: #7e7d80;
}
.numpad:active {
  background-color: #9e9e9f;
}
.operator {
  @apply text-center py-4 outline-none;
  background-color: orange;
}
.operator:active {
  background-color: #c97d20;
}
.zero {
  @apply col-span-2;
}
</style>
