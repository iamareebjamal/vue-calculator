<template>
  <div class="calculator">
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
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
}

.zero {
  grid-column: 1 / 3;
}

.btn {
  cursor: pointer;
  background-color: #f2f2f2;
  border: 1px solid #999;
}

.operator {
  background-color: orange;
  color: white;
}
</style>
