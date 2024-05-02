<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expense="+expense" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDelete"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>

</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed } from 'vue';
import { useToast } from 'vue-toastification';
const transactions = ref([]);
const toast=useToast()
//Get Total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
})

//Get Income
const income = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount > 0).
    reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0).toFixed(2)
})

//Get Expenses
const expense = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount < 0).
    reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0).toFixed(2)
})

//Add Transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({ id: generateUniqueId(), text: transactionData.text, amount: transactionData.amount })
toast.success("Transaction Added")
}

//Generate UniqueId
const generateUniqueId=()=>{
  return Math.floor(Math.random()*10000)
}

//Delete Transaction
const handleTransactionDelete=(id)=>{
transactions.value=transactions.value.filter((transaction)=>transaction.id!==id)
toast.success('Transaction Deleted')
}
</script>