<template>
  <div id="app" class="gradient-background d-flex justify-content-center align-items-center">
    <div class="justify-content-center mt-4">
      <div class="col-md-12">
        <div class="card bg-c-blue order-card">
          <div class="card-block">
            <h6 class="m-b-20">Quantidade</h6>
            <input v-model="amount" type="number" class="form-control" />
          </div>
        </div>
      </div>

      <div class="col-md-12">
        <div class="card bg-c-green order-card">
          <div class="card-block">
            <h6 class="m-b-20">De</h6>
            <select v-model="fromCurrency" class="form-control">
              <option v-for="currency in currencies" :key="currency.code" :value="currency.code">
                <i :class="'flag-icon-' + currency.flag"></i>
                {{ currency.name }}
              </option>
            </select>
          </div>
        </div>
      </div>

      <div class="col-md-12">
        <div class="card bg-c-yellow order-card">
          <div class="card-block">
            <h6 class="m-b-20">Para</h6>
            <select v-model="toCurrency" class="form-control">
              <option v-for="currency in currencies" :key="currency.code" :value="currency.code">
                <i :class="'flag-icon-' + currency.flag"></i>
                {{ currency.name }}
              </option>
            </select>
          </div>
        </div>
      </div>

      <div class="col-md-12">
        <div class="card bg-c-pink order-card">
          <div class="card-block">
            <p class="mt-3">Resultado: {{ currencySymbols[toCurrency] }} {{ result }}</p>
            <button @click="convert" class="btn btn-primary">Converter</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      amount: 1,
      fromCurrency: 'USD',
      toCurrency: 'BRL',
      currencies: [
        { code: 'USD', name: 'Dólar Americano', flag: 'us' },
        { code: 'BRL', name: 'Real Brasileiro', flag: 'br' },
        { code: 'CAD', name: 'Dólar Canadense', flag: 'ca' },
        { code: 'EUR', name: 'Euro', flag: 'eu' },
        { code: 'AUD', name: 'Dólar Australiano', flag: 'au' },
      ],
      currencySymbols: {
        USD: '$',
        BRL: 'R$',
        CAD: '$',
        EUR: '€',
        AUD: '$',
      },
      result: null,
    };
  },
  methods: {
    convert() {
      const apiURL = `http://economia.awesomeapi.com.br/json/last/${this.fromCurrency}-${this.toCurrency}`;

      this.result = null;

      axios
        .get(apiURL)
        .then((response) => {
          const rate = response.data[`${this.fromCurrency}${this.toCurrency}`].bid;
          this.result = (this.amount * rate).toFixed(2);
        })
        .catch((error) => {
          console.error('Erro ao converter:', error);
        });
    },
  },
};
</script>

<style scoped>
html, body {
  height: 100%;
  margin: 0;
}

.gradient-background {
  background: radial-gradient(circle, rgba(255,255,255,1) 0%, rgba(154,193,255,1) 100%); 
  min-height: 100vh;
}

.order-card {
  color: #fff;
  border-radius: 5px;
  -webkit-box-shadow: 0 1px 2.94px 0.06px rgba(4, 26, 55, 0.16);
  box-shadow: 0 1px 2.94px 0.06px rgba(4, 26, 55, 0.16);
  border: none;
  margin-bottom: 30px;
  -webkit-transition: all 0.3s ease-in-out;
  transition: all 0.3s ease-in-out;
}

.order-card .card-block {
  padding: 25px;
}

.order-card i {
  font-size: 26px;
}

.f-left {
  float: left;
}

.f-right {
  float: right;
}

.bg-c-blue {
  background: linear-gradient(135deg, #4c88ff, #6b9eff);
}

.bg-c-green {
  background: linear-gradient(135deg, #49e2b9, #68f4cf);
}

.bg-c-yellow {
  background: linear-gradient(135deg, #ffa64d, #ffc680);
}

.bg-c-pink {
  background: linear-gradient(135deg, #ff6688, #ff8f9a);
}
</style>