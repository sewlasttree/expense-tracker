<template>
  <Header />

  <div class="container">

    <Balance :total="+total" />
    <Income :income="+income" :expenses="+expenses"/>
    <History :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
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

  import {ref, computed, onMounted} from 'vue';

  const toast = useToast();

  const transactions = ref ([
    // {id: 1, text: 'gaji', amount: 5000000},
    // {id: 2, text: 'tas', amount: -150000},
    // {id: 3, text: 'nesting', amount: -100000},
  ]);

  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if(savedTransactions) {
      transactions.value = savedTransactions;
    }
  });
  
  // total 
  const total = computed(()=> {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0); 
  });

  // income
  const income = computed(()=> {
    return transactions.value.filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2); 
  });
  // expense
  const expenses = computed(()=> {
    return transactions.value.filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2); 
  });

  // addTransaction
  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    });

    saveTransactionsToLocalStorage();

    toast.success('transaction added');
  };


  // generate unique ID
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000 );
  };

  // delete transaction
  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(
      (transaction) => transaction.id !== id
    );

    saveTransactionsToLocalStorage();

    toast.success('transaction deleted');
  }

  // save to local storage
  const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  }
</script>