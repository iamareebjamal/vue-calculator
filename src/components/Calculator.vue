<template>
  <div class="calculator">
    <div class="header">
      <span class="mac close"></span>
      <span class="mac min"></span>
      <span class="mac max"></span>
      <span class="title">Calculator</span>
    </div>
    <div class="grid">
      <div class="display">{{current || '0'}}</div>
      <div @click="clear" class="btn">C</div>
      <div @click="sign" class="btn">+/-</div>
      <div @click="percent" class="btn">%</div>
      <div @click="divide" class="btn operator">÷</div>
      <div @click="append('7')" class="btn">7</div>
      <div @click="append('8')" class="btn">8</div>
      <div @click="append('9')" class="btn">9</div>
      <div @click="multiply" class="btn operator">x</div>
      <div @click="append('4')" class="btn">4</div>
      <div @click="append('5')" class="btn">5</div>
      <div @click="append('6')" class="btn">6</div>
      <div @click="subtract" class="btn operator">-</div>
      <div @click="append('1')" class="btn">1</div>
      <div @click="append('2')" class="btn">2</div>
      <div @click="append('3')" class="btn">3</div>
      <div @click="add" class="btn operator">+</div>
      <div @click="append('0')" class="btn zero">0</div>
      <div @click="dot"  class="btn">.</div>
      <div @click="equal" class="btn operator">=</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: '',
      previous: null,
      operatorCallback: null,
      operatorClicked: false
    }
  },

  methods: {
    clear() {
      this.current = ''
      this.operatorCallback = null
      this.previous = null
    },

    sign() {
      this.current = this.current.charAt(0) == '-' ? 
        this.current.slice(1) : `-${this.current}`
    },

    percent() {
      this.current = `${parseFloat(this.current) / 100}`
    },

    append(number) {
      if (number === '0' && this.current === '') {
        return
      }
      if (this.operatorClicked) {
        this.current = ''
        this.operatorClicked = false
      }
      this.current = `${this.current}${number}`
    },

    dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.')
      }
    },

    operator(callback) {
      this.equal()
      this.operatorCallback = callback
      this.operatorClicked = true
      this.previous = this.current
    },

    divide() {
      this.operator((a, b) => a / b)
    },

    multiply() {
      this.operator((a, b) => a * b)
    },

    subtract() {
      this.operator((a, b) => a - b)
    },

    add() {
      this.operator((a, b) => a + b)
    },

    equal() {
      if (!this.operatorCallback || this.operatorClicked || !this.previous)
        return
      this.current = `${this.operatorCallback(
        parseFloat(this.previous), parseFloat(this.current)
      )}`
      this.operatorCallback = null
      this.operatorClicked = true
    }
  }
}
</script>

<style scoped>
.calculator {
  margin: 0 auto;
  width: 400px;
  border-radius: 15px;
  border: 1px solid rgba(0, 0, 0, 0.3);
  overflow: hidden;
  -webkit-box-shadow: 0px 0px 20px 1px rgba(0,0,0,0.3);
  -moz-box-shadow: 0px 0px 20px 1px rgba(0,0,0,0.3);
  box-shadow: 0px 0px 15px 1px rgba(0,0,0,0.3);
}

.calculator > .grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  font-size: 35px;
}

.header {
  display: flex;
  align-items: center;
  height: 30px;
  background: linear-gradient(0deg, rgba(215,215,215,1) 0%, rgba(227,227,227,1) 35%, rgba(241,241,241,1) 100%);
}

.header:first-child {
  padding-inline-start: 5px;
}

.mac {
  background-color: greenyellow;
  height: 16px;
  width: 16px;
  margin: 0 3px;
  border-radius: 10px;
}

.close {
  background-color: #e23838;
}

.max {
  background-color: #5ebd3e;
}

.min {
  background-color: #ffb900;
}

.title {
  position: absolute;
  left: 0;
  right: 0;
}

.display {
  grid-column: 1 / 5;
  font-size: 60px;
  text-align: end;
  padding-inline-end: 10px;
  background-color: #555;
  color: white;
}

.zero {
  grid-column: 1 / 3;
}

.btn {
  cursor: pointer;
  display: flex;
  justify-content: center; /* align horizontal */
  align-items: center; /* align vertical */
  background-color: #f2f2f2;
  border: 1px solid #ccc;
}

.operator {
  background-color: orange;
  color: white;
  border: 1px solid rgba(0, 0, 0, 0.3);
}

.btn:active {
  background-color: #ddd;
}

.operator:active {
  background-color: darkorange;
}
</style>
