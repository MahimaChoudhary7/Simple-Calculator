<template>
  <div class="page">
  <div class="calculator">
    <div class="display">
      <div class="expression">{{ previous }} {{ operation }} {{ current }}</div>
      <div class="result">{{ current || '0' }}</div>
    </div>
    <div class="buttons">
      <button class="arm" @click="setOperation('+')">+</button>
      <button class="arm" @click="setOperation('-')">-</button>
      <button class="arm" @click="setOperation('*')">x</button>
      <button class="arm" @click="setOperation('/')">÷</button>

      <button @click="appendNumber('7')">7</button>
      <button @click="appendNumber('8')">8</button>
      <button @click="appendNumber('9')">9</button>
      <button class="equal" @click="calculate">=</button>

      <button @click="appendNumber('4')">4</button>
      <button @click="appendNumber('5')">5</button>
      <button @click="appendNumber('6')">6</button>

      <button @click="appendNumber('1')">1</button>
      <button @click="appendNumber('2')">2</button>
      <button @click="appendNumber('3')">3</button>

      <button @click="appendNumber('0')">0</button>
      <button @click="appendDot('.')">.</button>
      <button @click="clear">CE</button>
    </div>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      current: '',
      previous: '',
      operation: null,
    };
  },
  methods: {
    appendNumber(num) {
      if (num === '0' && this.current === '0') return;
      this.current += num;
    },

    appendDot() {
      if(!this.current.includes('.')) {
      this.current += '.';
    }
  },

  clear() {
    this.current = '';
    this.previous = '';
    this.operation = null;
  },

  setOperation(op) {
    if (this.current === '' && this.previous !== '') return;
    this.calculate();
    this.operation = op;
    this.previous = this.current;
    this.current = '';
  },

  calculate() {
    const prev = parseFloat(this.previous);
    const curr = parseFloat(this.current);
    if (isNaN(prev) || isNaN(curr)) return;

    let result;
    switch (this.operation) {
      case '+':
        result = prev + curr;
        break;

      case '-':
        result = prev - curr;
        break;

      case '*':
        result = prev * curr;
        break;

      case '/':
        result = curr === 0 ? 'Error' : prev/curr;
        break;

      default:
        return;
    }

    this.current = result.toString();
    this.previous = '';
    this.operation = null;
    },
  },
};
</script>

<style scoped>
.page {
  background: linear-gradient(#b2e2f2, #88c9e6);
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.calculator {
  width: 420px;
  border-radius: 20px;
  overflow: hidden;
  font-family: Arial, Helvetica, sans-serif;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  background: #f5f5f5;
}

.display {
  background: #1e1e1e;
  color: white;
  font-size: 48px;
  padding: 20px;
  text-align: right;
  word-wrap: break-word;
}

.expression {
  font-size: 20px;
  color: #ccc;
  text-align: right;
  padding: 0 20px;
}

.result {
  font-size: 48px;
  color: white;
  text-align: right;
  padding: 0 20px;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 1fr;
}

button {
  padding: 20px;
  font-size: 25px;
  border: 1px solid #ddd;
  background: white;
  cursor: pointer;
  transition: background 0.2s;
}

button:hover {
  background: #eee;
}

.arm {
  background: #f0f0f0;
  font-weight: bold;
}

.arm:hover {
  background: #ddd;
}

.equal {
  grid-row: span 4;
  background: #ff7a3c;
  color: white;
  font-weight: bold;
  border-left: 2px solid #ddd;
}
</style>
