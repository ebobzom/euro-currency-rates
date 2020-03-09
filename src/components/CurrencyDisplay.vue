<template>
  <div class="currency">
    <p>Base: <span>{{ base }}</span>, Date: <span>{{ date }}</span></p>
    <ul class='main'>
      <div class='currency-list' v-for='(val,index) in currencyData' v-bind:key="index">
        <li class="val-key">{{ val.key }}</li>
        <li> &lt; == &gt;</li>
        <li class="val-property">{{ val.digit }}</li>
      </div>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

function mutateData(data) {
  const keys = Object.keys(data);
  const values = Object.values(data);
  const result = [];

  keys.forEach((key, index) => {
    result.push({ key, digit: values[index] });
  });
  return result;
}

export default {
  name: 'CurrencyDisplay',
  data() {
    return {
      currencyData: [],
      base: '',
      date: '',
    };
  },
  created() {
    axios.get('https://api.exchangeratesapi.io/latest')
      .then((res) => {
        this.base = res.data.base;
        this.date = res.data.date;
        this.currencyData = mutateData(res.data.rates);
      });
  },

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
    body{
      background-color: rgb(216, 233, 233);
    }

    p{
      text-align: center;
      font-weight: 900;
      font-size: 42px;
      color: rgb(117, 117, 163);
    }

    .main{
      width: 50%;
      margin: 0 auto;
      background-color: azure;
    }

    .val-key{
      font-weight: 600;
      color: rgb(75, 75, 158);
    }

    .val-property{
      font-weight: 600;
      color: rgb(184, 77, 77);
    }

    li{
      list-style-type: none;
      display: inline-block;
      padding: 10px;
      margin: 30px;
    }

    .currency-list{
      margin: 0 auto;
      text-align: center;
    }

    @media screen and (max-width: 770px) {
      li{
      list-style-type: none;
      display: inline-block;
      padding: 5px;
      margin: 5px;
    }

  }
</style>
