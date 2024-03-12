<template>
  <div>
    <label>Enter infura api key</label>
    <input type="text" v-model="INFURA_API_KEY" placeholder="Enter infura api key" />
    <label>Enter token contract address</label>
    <input type="text" v-model="tokenContract" placeholder="Enter token contract address" />
    <label>Enter holder's address</label>
    <input type="text" v-model="holderAddress" placeholder="Enter holder's address" />
    <button @click="fetchData">Fetch Data</button>
    <p>USDT Balance: {{ usdtBalance }}</p>
    <p>Latest Ethereum Block: {{ latestBlockNumber }}</p>
  </div>
</template>

<script lang="ts">
import { getLastBlockNumber } from '@noor4app/ethereum-info/blockNumber.js';
import { getTokenBalance } from '@noor4app/ethereum-info/contractInteraction.js';

export default {
  data() {
    return {
      usdtBalance: 'Loading...',
      latestBlockNumber: 0,
      INFURA_API_KEY: '',
      tokenContract: '',
      holderAddress: '',
    };
  },
  methods: {
    async fetchData() {
      if (!this.INFURA_API_KEY || !this.tokenContract || !this.holderAddress) {
        alert('Please enter all required fields.');
        return;
      }
      try {
        this.usdtBalance = await getTokenBalance(this.holderAddress, this.INFURA_API_KEY, this.tokenContract);
        this.latestBlockNumber = await getLastBlockNumber(this.INFURA_API_KEY);
      } catch (error) {
        console.error('Failed to fetch data:', error);
        this.usdtBalance = 'Error fetching USDT balance';
        this.latestBlockNumber = 0;
      }
    },
  },
};
</script>

<style scoped>
/* Basic styling for the form */
div {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* Styling for the input fields */
input[type="text"] {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #ccc;
  border-radius: 4px;
}

/* Styling for the button */
button {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #0056b3;
}

/* Styling for the paragraphs */
p {
  margin-top: 20px;
  font-size: 18px;
}
</style>