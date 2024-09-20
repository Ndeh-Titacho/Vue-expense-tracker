<template>
  <Header />
  <div class="container">
    <balance :total="+total"/>
    <IncomeExpense :income="+income" :expense="+expense"/>
    <TransactionHistory :transactions="transactions" @transactionDeletion="handleTransactionDeletion"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>



<script setup>
import Header from '@/components/Header.vue';
import balance from '@/components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionHistory from './components/TransactionHistory.vue';
import AddTransaction from './components/AddTransaction.vue';

import {ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions = ref( []);

//Populate transactions array with data from localstorage
onMounted(() =>{
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if(savedTransactions){
    transactions.value = savedTransactions;
  }
});

//Get Balance
const total = computed(() => {
  return transactions.value
  .reduce((acc, transaction) => acc + transaction.amount, 0)
  .toFixed(2);
});

//Get income

// const incom = computed(()=> {
//   let ans;
//   transactions.value.filter((transaction) => {
//     let ans =  transaction.amount > 0;
//     return ans;
//   });
//   ans.reduce((acc , transaction) => {
//     let ans1 = acc + transaction.amount;
//     return ans1.toFixed(2);
//   });
// })
const income = computed(()=> {
  return transactions.value
  .filter((transaction) => transaction.amount > 0)
  .reduce((acc, transaction) => acc + transaction.amount,0)
  .toFixed(2);

});

//Get expense
const expense = computed(()=> {
  return transactions.value
  .filter((transaction) => transaction.amount < 0)
  .reduce((acc, transaction) => acc + transaction.amount,0)
  .toFixed(2);

});

// Add Transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    expense: transactionData.text,
    amount: transactionData.amount,

  });

  saveTransactionsToLocalStorage();

  toast.success('Transaction Added.');
};

//generate uique ID for new entries
const generateUniqueId = () => {
  return Math.floor(Math.random() *1000);
};

//Delete a transaction
const handleTransactionDeletion = (id) => {
  transactions.value = transactions.value
  .filter((transaction) => transaction.id !== id);
  
saveTransactionsToLocalStorage();

  toast.warning('Transaction Deleted.');
};

//Save to local storage
const saveTransactionsToLocalStorage = () => {
 localStorage.setItem('transactions',JSON.stringify(transactions.value));
}
</script>

