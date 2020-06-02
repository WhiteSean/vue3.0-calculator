<template>
  <div class="home">
    <div class="container">
      <input id="input-box" type="text" size="21" maxlength="21" v-model="result" readonly="readonly">
      <div class="btn-list">
        <div @click="clearInput()" class="btn-item btn-two clear-margin-left color-red">C</div>
        <div @click="inputValue('%')" class="btn-item color-blue">%</div>
        <div @click="backInput()" class="btn-item btn-two color-red">←</div>
        <div @click="inputValue('7')" class="btn-item clear-margin-left">7</div>
        <div @click="inputValue('8')" class="btn-item">8</div>
        <div @click="inputValue('9')" class="btn-item">9</div>
        <div @click="squareValue()" class="btn-item color-blue">×²</div>
        <div @click="radicalValue()" class="btn-item color-blue">√</div>
        <div @click="inputValue('4')" class="btn-item clear-margin-left">4</div>
        <div @click="inputValue('5')" class="btn-item">5</div>
        <div @click="inputValue('6')" class="btn-item">6</div>
        <div @click="inputValue('×')" class="btn-item color-blue">×</div>
        <div @click="inputValue('÷')" class="btn-item color-blue">÷</div>
        <div @click="inputValue('1')" class="btn-item clear-margin-left">1</div>
        <div @click="inputValue('2')" class="btn-item">2</div>
        <div @click="inputValue('3')" class="btn-item">3</div>
        <div @click="inputValue('+')" class="btn-item color-blue">+</div>
        <div @click="inputValue('-')" class="btn-item color-blue">-</div>
        <div @click="inputValue('0')" class="btn-item btn-two clear-margin-left">0</div>
        <div @click="inputValue('.')" class="btn-item">.</div>
        <div @click="calValue()"  class="btn-item btn-two color-red">=</div>
      </div>
    </div>

  </div>
</template>

<script>
import {ref} from 'vue'

export default {
  name: 'Home',
  setup(){
    // eslint-disable-next-line no-unused-vars
    const result = ref('0')
    const inputValue = (param) => {
      // eslint-disable-next-line no-debugger
      /*debugger
      if(Object.prototype.toString.call(result.value) === "[object Number]"){   //判断输入框内容是否为数字类型
        result.value = "0";   //数字类型说明是上个计算结果,清空内容
      }*/
      let str ='' + result.value; //输入内容时,将输入框内容转为字符串类型
      let len = str.length;
      let arr = ["+","-","×","÷"];
      let num = (''+parseFloat(str.split('').reverse().join(''))).split('').reverse().join('');   //parseInt(str.split('').reverse().join('')))是获取输入框内最后一串数字,再反转回来 ,num为输入框内最后一串数字
      let nlen = num.length;
      if((num!== '0' && param !== '.')|| (param === '.'&& num.indexOf(".") === -1)){   //输入框内最后一串数字不为0时拼接字符串
        if(arr.indexOf(str.charAt(len-1)) !== -1 && arr.indexOf(param) !== -1){    //若一开始输入内容为运算符,输入无效
          return;
        }
        result.value += param;   //拼接输入内容
      }else{
        arr.push("%");
        if(param === '.'){      //若num中已有小数点,输入内容为小数点,视为无效
          return;
        }else if(!(arr.indexOf(param) !== -1)){    //判断输入框内最后一个字符不为运算符
          result.value =str.substring(0,str.length-nlen) + param;  //输入框内最后一串数字为0时,删除0拼接
        }
      }
    };
    const clearInput = () => {
      result.value = '0'
    };
    const  backInput = () => {
      // eslint-disable-next-line no-debugger
      let str = result.value.toString();
      if(str.length === 1){
        result.value = "0";
      }else{
        result.value = str.slice(0,str.length-1);
      }
    };
    const squareValue = () => {
      let str = result.value;
      result.value = Math.pow(eval(str),2)
    };
    const radicalValue = () => {
      let str = result.value;
      result.value = Math.sqrt(eval(str));
    };
    const calValue = () => {
      let str = result.value;
      str = str.replace('×','*').replace('÷','/').replace('%','*0.01');    //替换运算符
      result.value = eval(str);      //开始计算
    }
    return{
      result,
      inputValue,
      clearInput,
      backInput,
      squareValue,
      radicalValue,
      calValue
    }
  }
}
</script>
<style scoped lang="scss">
  .container{
    display: inline-block;
    border: 1px solid #CCC;
    box-sizing: border-box;
    padding: 20px;
    input{
      width: 290px;
      height: 40px;
      text-align: right;
      padding: 0;
      box-sizing: border-box;
    }
    .btn-list{
      display: flex;
      flex-wrap: wrap;
      width: 290px;
      .btn-item{
        width: 50px;
        height: 50px;
        border: 1px solid #CCC;
        margin: 10px 0 0 10px;
        box-sizing: border-box;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        background:-webkit-gradient(linear, 0 0, 0 100%, from(#f7f7f7), to(#ebebeb));
        &:active{
          background: #FFF;
        }
      }
      .btn-two{
        width: 110px;
      }
      .clear-margin-left{
        margin-left: 0;
      }
      .color-red{color:#ff5050}
      .color-blue{color:#00b4ff}
    }
  }
</style>
