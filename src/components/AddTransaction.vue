<template>
    <h3>Add new transaction</h3>
      <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
          <label for="text">Text</label>
          <input type="text" v-model="text"  id="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
          <label for="amount"
            >Amount <br />
            (negative - expense, positive - income)</label
          >
          <input type="number" v-model="amount" id="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
      </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';


//Get toast interface
const toast = useToast();

//Create an emit event

const emit = defineEmits(['transactionSubmitted']);



const text = ref('');
const amount = ref('');

const onSubmit =() => {

  if( !text.value || !amount.value){
    toast.error("All fields must be filled!");
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
   };

   emit('transactionSubmitted', transactionData);
    


  //clear form fields
  text.value = '';
  amount.value = '';

}
</script>