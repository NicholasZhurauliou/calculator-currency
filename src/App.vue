<template>
  <div id="app">
    <label>
      BYN:
      <input type="text" 
             :value="valueBYN" 
            @input="calculate($event.target.value, 'BYN')" 
      />
    </label>
    <label>
      USD:
      <input type="text" 
             :value="valueUSD" 
            @input="calculate($event.target.value, 'USD')" 
      />
    </label>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      valueBYN: '',
      valueUSD: ''
    }
  },
  methods: {
    calculate(value, currency) {
      const regexp = /^\d+([.,]\d*)?$/;
      const rate  = 3;
      const percent = 5;
      const multiplier = 1 - percent / 100;
    
      if (value === '') {
        this.valueUSD = '';
        this.valueBYN = '';
      } else if (!regexp.test(value)) {
        this.$forceUpdate();
      } else {
        const newValue = +value.replace(',', '.'); 
    
        switch(currency) {
          case 'USD': {
            this.valueUSD = newValue;
            this.valueBYN = newValue * multiplier * rate;
            break;
          }
          case 'BYN': {
            this.valueBYN = newValue;
            this.valueUSD = newValue * multiplier / rate;
            break;
          }
        }
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
#app label {
  display: block;
  padding: 1rem;
}
#app input[type="text"]::-ms-clear {
  display: none;
}
#app input[type="text"]::-webkit-clear-button{
  -webkit-appearance: none;
  margin: 0;
}
</style>
