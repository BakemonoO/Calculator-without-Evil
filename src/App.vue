<template>
  <div id="app" class="app">
    <div class="calculatorValue">
      <div class="previous area"> {{ textArea }} </div>
      <div 
      class="calculatorValue-area area"
      v-if="calculatorValue === ''">
      {{ result || '0' }}
      </div>

      <div 
      class="calculatorValue-area area"
      v-else>
      {{ calculatorValue }}
      </div>
     
      <div class="buttons">
        <button v-for="btn in btns" 
        class="btns"
        :key="btn"
        :class="{'number-button': !isNaN(btn) || ['.', '<='].includes(btn), 'operators': ['/', 'x', '+', '-', '='].includes(btn)}"
        @click="pick(btn)"
        > {{ btn }} </button>
      </div>
    </div>
  </div>
</template>

<script>

  export default {
    data () {
      return {
        btns: ['C', '+/-', '%', '/', 7, 8, 9, 'x', 4, 5, 6, '-', 1, 2, 3, '+', '.', 0, '<=', '='],
        calculatorValue: '',
        textArea: '',
        previousValue: null,
        operator: null,
        result: null,
        preOperatorTrue: null,
        preOperatorFalse: null,
        preNumber: null,
      }
    },
    methods: {
      pick(btn) {
        if (!isNaN(btn) || btn === '.') {
          this.calculatorValue += btn
          this.preNumber = this.calculatorValue
         } else if (btn === '<=') {
          this.calculatorValue = this.calculatorValue.split('').filter((x, i, arr) => i !== (arr.length - 1)).join('')
          this.preNumber = this.calculatorValue
        } else if (btn === 'C') {
          this.calculatorValue = ''
          this.textArea = ''
          this.previousValue = null,
          this.operator = null
          this.result = null
        } else if (['/', 'x', '+', '-', '%'].includes(btn)) {
            this.operator = btn
            if (this.previousValue === null) {
            this.previousValue = this.calculatorValue
            this.calculatorValue = ''
            this.textArea = this.previousValue + ' ' + this.operator + ' '
        } else {
        if (this.operator === '+') {
          if (this.result !== null) {
              this.previousValue = this.result
              this.preNumber = this.calculatorValue
              this.calculatorValue = ''
            }
            this.result = +this.previousValue + +this.preNumber
            this.previousValue = +this.previousValue + +this.preNumber
            this.textArea = this.result + ' ' + this.operator + ' '
            this.calculatorValue = ''
            
          } else if (this.operator === '-') {
          if (this.result !== null) {
              this.previousValue = this.result
              this.preNumber = this.calculatorValue
              this.calculatorValue = ''
            }
            this.result = +this.previousValue - +this.preNumber
            this.previousValue -= +this.preNumber
            this.textArea = this.result + ' ' + this.operator + ' '
            this.calculatorValue = ''
            
          } else if (this.operator === 'x') {
            if (this.result !== null) {
              this.previousValue = this.result
              this.textArea = this.previousValue + ' ' + this.operator + ' ' 
            } else {
              this.result = this.previousValue * this.preNumber
            this.previousValue *= this.preNumber
            this.textArea = this.result + ' ' + this.operator + ' '
            this.calculatorValue = ''
            }
          } else if (this.operator === '%') {
            if (this.result !== null) {
              this.previousValue = this.result
              this.preNumber = ''
            }
            this.result = +this.previousValue * (+this.preNumber / 100)
            this.previousValue = +this.previousValue * (+this.preNumber / 100)
            this.textArea = this.result + ' ' + this.operator + ' '
            this.calculatorValue = ''
          } else {
            if (this.result !== null) {
              this.previousValue = this.result
              this.textArea = this.previousValue + ' ' + this.operator + ' ' 
            } else {
              this.result = this.previousValue / this.preNumber
            this.previousValue /= this.preNumber
            this.textArea = this.result + ' ' + this.operator + ' '
            this.calculatorValue = ''
            }
          
          }
        }
      } else if (btn === '=') {
        if (this.previousValue === null) {
          this.textArea = 'Выберите операцию и число'
        } else {
          if (this.operator === '+') {
        if (this.result === null) {
          this.result = +this.previousValue + +this.preNumber
          this.calculatorValue = ''
          this.textArea = this.previousValue + ' ' + this.operator + ' ' + this.preNumber + ' ' + '='
        } else {
          this.previousValue = this.result 
          this.result += +this.preNumber
          this.textArea = this.previousValue + ' ' + this.operator + ' ' + this.preNumber + ' ' + '='
          this.calculatorValue = ''
        } 
      } else if (this.operator === 'x') {
        if (this.result === null) {
          this.result = +this.previousValue * +this.preNumber
          this.calculatorValue = ''
          this.textArea = this.previousValue + ' ' + this.operator + ' ' + this.preNumber + ' ' + '='
        } else {
          this.previousValue = this.result 
          this.result *= +this.preNumber
          this.textArea = this.previousValue + ' ' + this.operator + ' ' + this.preNumber + ' ' + '='
          this.calculatorValue = ''
        } 
      } else if (this.operator === '-') {
        if (this.result === null) {
          this.result = +this.previousValue - +this.preNumber
          this.calculatorValue = ''
          this.textArea = this.previousValue + ' ' + this.operator + ' ' + this.preNumber + ' ' + '='
        } else {
          this.previousValue = this.result 
          this.result -= +this.preNumber
          this.textArea = this.previousValue + ' ' + this.operator + ' ' + this.preNumber + ' ' + '='
          this.calculatorValue = ''
        } 
      } else if (this.operator === '%') {
          this.result = +this.previousValue * (+this.preNumber / 100)
          this.calculatorValue = ''
          this.textArea = this.previousValue + ' ' + this.operator + ' ' + this.preNumber + ' ' + '=' 
      } else {
        if (this.result === null) {
          this.result = +this.previousValue / +this.preNumber
          this.calculatorValue = ''
          this.textArea = this.previousValue + ' ' + this.operator + ' ' + this.preNumber + ' ' + '='
        } else {
          this.previousValue = this.result 
          this.result /= +this.preNumber
          this.textArea = this.previousValue + ' ' + this.operator + ' ' + this.preNumber + ' ' + '='
          this.calculatorValue = ''
        } 
      }
        }
        
    } else if (btn === '+/-') {
      if (this.result !== null) {
        this.result = -this.result
      } else {
        this.calculatorValue = -this.calculatorValue
      this.preNumber = this.calculatorValue
      }
    }
  },
    },

    computed: {
    }
  }
  
</script>

<style>

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

  .app {
    background: rgb(112, 102, 102);
    display: flex;
    justify-content: center;
    align-content: center;
    height: 100vh;
  }

  .calculatorValue {
    width: 300px;
    height: 500px;
    background: #17171C;
    margin-top: 100px;
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 5px solid #17171C;
    border-radius: 15px;
  }

  .area {
    width: 250px;
    height: 50px;
    background: #17171C;
    font-size: 30px;
    color: white;
    display: flex;
    justify-content: end;
    align-items: center;
  }

  .previous {
    margin-top: 25px;
    align-items: flex-end;
    color: darkgray;
    font-size: 15px;
  }

  .buttons {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    width: 250px;
    margin-top: 25px;
  }

  .btns {
    width: 20%;
    height: 50px;
    background: #4E505F;
    color: white;
    font-size: 18px;
    border: 1px solid #4E505F;
    border-radius: 15px;
    margin: 5px;
  }

  .btns:hover {
    background: rgb(60, 138, 226);
    border-color: rgb(60, 138, 226);
  }

  .number-button {
    background: #2E2F38;
    border-color: #2E2F38;
  }

  .operators {
    background: #4B5EFC;
    border-color: #4B5EFC;;
  }
</style>