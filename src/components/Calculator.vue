<template>
  <div class="p-3 container" >
    <!--Calculator Result-->
    <div class="result rounded m-1 p-4 w-full text-right text-white font-weight-bold ">
      {{calculatorValue || 0}}
    </div>

     <!--Calculator Buttons-->
    <div class="row g-0">
      <div v-for="(element) in calculatorElements" :key="element" :class="element===0 ? 'col-9' : 'col-3'" >
        <div class="lead text-white text-center m-1 py-3 rounded hover_button" 
        :class="isNaN(element) ? 'green_bt' : 'dark_bt'"
        @click="action(element)"
        >
           {{element}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  props: {
    msg: String
  },

  data(){
    return {
      calculatorValue: '',
      operator: null,
      previousCalculatorValue:'',
      totalValue:'',
      afterOperation: false,
      calculatorElements:['C','*','/','-',7,8,9,'+',4,5,6,'%',1,2,3,'=',0,'.'],
    }
  },
  methods:{
      action(el){
      if(this.operator && this.afterOperation){
          this.calculatorValue = ''; 
      }
      if ((this.calculatorValue.toString().indexOf('.') > -1) && el === '.' && !this.operator ) {
        return;
      }
      //Append the value
       if (!isNaN(el) || el === '.') {
         if(this.calculatorValue === '' && el === '.'){
            this.calculatorValue += 0 +  '';
         }
         this.calculatorValue += el + '';
         this.afterOperation = false;
               console.log(this.calculatorValue);
       } 
      //Clear value
        if (el === 'C') {
          this.calculatorValue = '';
        }
        if (el === '%') {
          this.calculatorValue =  this.calculatorValue/100 ;
        }
        if (['*','/','-','+'].includes(el)) {
          this.operator = el;
          this.previousCalculatorValue = this.calculatorValue;
          this.afterOperation = true;
        }
        if (el === '=') {
          this.totalValue = Function('"use strict";return (' + this.previousCalculatorValue + this.operator + this.calculatorValue + ')')();
          this.calculatorValue = this.totalValue;
          this.previousCalculatorValue = '';
          this.operator = null;
          this.afterOperation = true;
        } 
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container{
  max-width: 400px;
  margin:50px auto;
  background: #234;
}
.result{
  background: #31475e;
  text-align: right;
  font-weight: bold;
}
.dark_bt{
   background: #31475e;
}
.green_bt{
   background: #3fb984;
}
.hover_button:hover{
  cursor: pointer;
  background: #3D5875;
}
</style>
