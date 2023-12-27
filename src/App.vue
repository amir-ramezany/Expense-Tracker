<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expense="+expense" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";
// export default {
//   name: "App",
//   components: {
//     Header,
//     Balance,
//     IncomeExpense,
//     TransactionList,
//     AddTransaction,
//   },
// };
const transactions = ref([]);

onMounted(() => {
  const saveTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (saveTransactions) {
    transactions.value = saveTransactions;
  }
});

//total
const total = computed(() => {
  return transactions.value.reduce((acc, item) => {
    return acc + item.amount;
  }, 0); //acc first value gonna be  0
});
//Income
const income = computed(() => {
  return transactions.value
    .filter((item) => item.amount > 0)
    .reduce((acc, item) => {
      return acc + item.amount;
    }, 0)
    .toFixed(2);
});
//Expense
const expense = computed(() => {
  return Math.abs(
    transactions.value
      .filter((item) => item.amount < 0)
      .reduce((acc, item) => {
        return acc + item.amount;
      }, 0)
  ).toFixed(2);
});

//Add transaction
const toast = useToast();
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  saveTransactionsToLocalStorage();
  toast.success("Transaction Added ");
};
//generate ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

//Delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((item) => item.id !== id);
  saveTransactionsToLocalStorage();
  toast.success("Transaction Deleted");
};
//save to localStorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
