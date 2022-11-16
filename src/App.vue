<template>
  <div id="app" class="app">
    <div class="toggle">
      <button 
      class="toggleBtn"></button>
    </div>
    
    <div class="calculatorValue">
      <div class="previous area"> {{ textArea }} </div>
      <div 
      class="area"
      v-if="calculatorValue === ''">
      {{ result || '0' }}
      </div>

      <div 
      class="area"
      v-else>
      {{ calculatorValue }}
      </div>
     
      <div class="buttons">
        <button v-for="btn in btns" 
        class="btns"
        :key="btn.id"
        :class="{'number-button': !isNaN(btn.name) || ['.', '<='].includes(btn.name), 'operators': ['/', 'x', '+', '-', 'C', '+/-', '%'].includes(btn.name)}"
        @click="!isNaN(btn.name) ? addNumber(btn.name) : ['.'].includes(btn.name) ? addDot(btn.name) : ['/', 'x', '+', '-', '%'].includes(btn.name) ? changeOperator(btn.name) : ['+/-', 'C', '<='].includes(btn.name) ? optionsCalc(btn.name) : getResult()"
        > {{ btn.name }} </button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        btns: [{name: 'C', type: 'operator', id: 1}, 
               {name: '+/-', type: 'operator', id: 2},
               {name: '%', type: 'operator', id: 3},
               {name: '/', type: 'operator', id: 4},
               {name: 7, type: 'number', id: 5},
               {name: 8, type: 'number', id: 6},
               {name: 9, type: 'number', id: 7},
               {name: 'x', type: 'operator', id: 8},
               {name: 4, type: 'number', id: 9},
               {name: 5, type: 'number', id: 10},
               {name: 6, type: 'number', id: 11},
               {name: '-', type: 'operator', id: 12},
               {name: 1, type: 'number', id: 13},
               {name: 2, type: 'number', id: 14},
               {name: 3, type: 'number', id: 15},
               {name: '+', type: 'operator', id: 16},
               {name: '.', type: 'operator', id: 17},
               {name: 0, type: 'number', id: 18},
               {name: '<=', type: 'operator', id: 19},
               {name: '=', type: 'operator', id: 20}
              ],
        calculatorValue: '',
        textArea: '',
        previousValue: null,
        operator: null,
        result: null,
        preNumber: null,
        darkTheme: true,
      }
    },
    methods: {

      addNumber(btn) {
        this.calculatorValue += btn
          this.preNumber = this.calculatorValue
      },

      addDot(btn) {
        if (this.calculatorValue.split('').filter(x => x === '.').length > 0) {
          return false
        } else {
          if (this.calculatorValue === '') {
          this.calculatorValue = 0 + btn
        } else {
          this.calculatorValue += btn
          this.preNumber = this.calculatorValue
        }
      }
    },

      changeOperator(btn) {
        this.operator = btn
            if (this.previousValue === null) {
            this.previousValue = this.calculatorValue
            this.calculatorValue = ''
            this.textArea = this.previousValue + ' ' + this.operator + ' '
            } else {
              if (this.result !== null) {
              this.previousValue = this.result
              this.preNumber = this.calculatorValue
              this.calculatorValue = ''
            }
              if (this.operator === '+') {
                this.sum()
              }
              if (this.operator === '-') {
                this.sub()
              }
              if (this.operator === 'x') {
                this.mul()
              }
              if (this.operator === '/') {
                this.div()
              }
              if (this.operator === '%') {
                this.percent()
              }
            }
      },

      sum() {
        this.result = +this.previousValue + +this.preNumber
        this.previousValue = +this.previousValue + +this.preNumber
        this.textArea = this.result + ' ' + this.operator + ' '
        this.calculatorValue = ''
      },

      sub() {
        this.result = +this.previousValue - +this.preNumber
        this.previousValue -= +this.preNumber
        this.textArea = this.result + ' ' + this.operator + ' '
        this.calculatorValue = ''
      },

      mul() {
        if (this.result !== null) {
              this.previousValue = this.result
              this.textArea = this.previousValue + ' ' + this.operator + ' ' 
            } else {
        this.result = this.previousValue * this.preNumber
        this.previousValue *= this.preNumber
        this.textArea = this.result + ' ' + this.operator + ' '
        this.calculatorValue = ''
            }
      },

      div() {
        if (this.result !== null) {
            this.previousValue = this.result
            this.textArea = this.previousValue + ' ' + this.operator + ' ' 
          } else {
        this.result = this.previousValue / this.preNumber
        this.previousValue /= this.preNumber
        this.textArea = this.result + ' ' + this.operator + ' '
        this.calculatorValue = ''
          }
      },

      percent() {
        this.previousValue = this.result
        this.result = +this.previousValue * (+this.preNumber / 100)
        this.textArea = this.previousValue + ' ' + this.operator + ' '
        this.calculatorValue = ''
      },
      
      getResult() {
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
        } else  {
          this.previousValue = this.result 
          this.result /= this.preNumber
          this.textArea = this.previousValue + ' ' + this.operator + ' ' + this.preNumber + ' ' + '='
          this.calculatorValue = ''
        } 
      }
    }
        
      },

      optionsCalc(btn) {
        if (btn === 'C') {
          this.calculatorValue = ''
          this.textArea = ''
          this.previousValue = null,
          this.operator = null
          this.result = null
        }
        if (btn === '+/-') {
          if (this.result !== null) {
        this.result = -this.result
      } else {
        this.calculatorValue = -this.calculatorValue
      this.preNumber = this.calculatorValue
      }
        }
        if (btn === '<=') {
          if (this.result) {
            this.result = this.result.toString().split('').filter((x, i, arr) => i !== (arr.length - 1)).join('')
          }
          this.calculatorValue = this.calculatorValue.split('').filter((x, i, arr) => i !== (arr.length - 1)).join('')
          this.preNumber = this.calculatorValue
        }
      }
    },

    computed: {
      thisTheme() {
        return this.darkTheme ? 'Dark' : 'Light'
      },
    
    },

    mounted() {
      let arrButtons = [1,2,3,4,5,6,7,8,9,0]
      let operators = ['+', '-', '/', '%']
      
      document.addEventListener(`keydown`, (event) => {
        for (let i = 0; i < arrButtons.length; i++) {
          if (event.key === `${arrButtons[i]}`) {
          this.addNumber(arrButtons[i])
          }
        }

        for (let i = 0; i < operators.length; i++) {
          if (event.key === `${operators[i]}`) {
            this.changeOperator(`${operators[i]}`)
          }
        }

        if (event.key === '.') {
          this.addDot('.')
        }

        if(event.key === '*') {
          this.changeOperator('x')
        }

        if(event.key === 'Enter') {
          this.getResult('=')
        }

        if(event.key === 'Backspace') {
          this.optionsCalc('<=')
        }

        if(event.key === 'Delete') {
          this.optionsCalc('C')
        }
      })
    },
  }
</script>

<style>

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

  .app {
    background: rgba(65, 114, 99, 0.9);
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100vh;
  }

  .calculatorValue {
    width: 300px;
    height: 500px;
   background: #262d37;
    margin-top: 75px;
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 5px solid #262d37;
    border-radius: 15px;
  }

  .area {
    width: 250px;
    height: 50px;
    background: #262d37;
    font-size: 30px;
    color: white;
    display: flex;
    justify-content: end;
    align-items: center;
    overflow: hidden;
  }

  .previous {
    margin-top: 40px;
    align-items: center;
    color: darkgray;
    font-size: 15px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  }

  .buttons {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    width: 250px;
    margin-top: 15px;
  }

  .btns {
    width: 20%;
    height: 50px;
    background: #ef864b;
    color: white;
    font-size: 18px;
    border: 1px solid #ef864b;
    border-radius: 15px;
    margin: 5px;
    box-shadow: 3px 5px 5px rgba(0, 0, 0, 0.25);
  }

  .btns:hover {
    background: #f09a68;
    border-color: #f09a68;
  }
  
  .btns:active {
    transform: scale(0.9);
    box-shadow: none;
  }

  .number-button {
    background: #262d37;
    border-color: #262d37;
  }

  .number-button:hover {
    background: #2e3744;
    border-color: #2e3744;
  }

  .operators {
    background: #367b8a;
    border-color: #367b8a;
  }

   .operators:hover {
    background: #4991a1;
    border-color: #4991a1;
   }

  .toggle {
    height: 50px;
    width: 100px;
    margin-top: 10px;
    background: #262d37;
    border: 1px solid #262d37;
    border-radius: 25px;
    display: flex;
    align-items: center;
    padding-left: 5px;
    margin-top: 50px;
  }

  .toggleBtn {
    width:  40px;
    height: 40px;
    background: #367b8a;
    border: 1px solid #367b8a;;
    border-radius: 20px;
  }

</style>