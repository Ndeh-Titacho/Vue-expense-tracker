<template>
     <h3>History</h3>
      <ul id="list" class="list">
        <li v-for="transaction in transactions" :key='transaction.id' :class="ExpenseType(transaction.amount)">
         {{ transaction.expense }} <span>${{ transaction.amount }}</span>
         <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
      </ul>
</template>

<script setup>
import { defineProps,computed } from 'vue';

const props = defineProps({
    transactions: {
        type: Array,
        required: true,
    }
});
//Determine color of expense and income
  const ExpenseType = (amount) =>{
    
    if(amount > 0){
        return 'plus';
    } else { 
        return 'minus';
    }
};

//Delete a transaction

const emit = defineEmits(['transactionDeletion']);
const deleteTransaction = (id) => {
    emit('transactionDeletion', id);
};
</script>