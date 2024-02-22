<template>
  <Header />

  <div class="container">

    <Balance :total="total" />
    <Income :income="+income" :expenses="+expenses"/>
    <History :transactions="transactions"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>

  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import Income from './components/Income.vue';
  import History from './components/History.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import {useToast} from 'vue-toastification';

  import {ref, computed} from 'vue';

  const toast = useToast();

  const transactions = ref ([
    {id:1, text: 'gaji', amount: 5000000},
    {id:1, text: 'tas', amount: -150000},
    {id:1, text: 'nesting', amount: -80000},
  ]);

  // total 
  const total = computed(()=> {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0); 
  });

  // income
  const income = computed(()=> {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2); 
  });
  // expense
  const expenses = computed(()=> {
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2); 
  });

  // addTransaction
  const handleTransactionSubmitted = (transactionData) => {
    transaction.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    });
    toast.success('transaction added')
  };


  // generate unique ID
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000 );
  };

</script>