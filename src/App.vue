<template>
  <Header />

  <div class="container">
    <Balance />
    <Income />
    <History :transactions="transactions"/>
    <AddTransaction />
  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import Income from './components/Income.vue';
  import History from './components/History.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import {ref} from 'vue';

  const transactions = ref ([
    {id:1, text: 'gaji', amount: 5000000},
    {id:1, text: 'tas', amount: -150000},
    {id:1, text: 'nesting', amount: -80000},
  ]);
</script>