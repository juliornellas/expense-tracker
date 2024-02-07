<script setup>
import Header from "@/components/Header.vue";
import Balance from "@/components/Balance.vue";
import IncomeExpenses from "@/components/IncomeExpenses.vue";
import TransactionList from "@/components/TransactionList.vue";
import TransactionForm from "@/components/TransactionForm.vue";

import { computed, onMounted, ref } from "vue";

import { useToast } from "vue-toastification";
const toast = useToast();

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const transactions = ref([
  // { id: 1, text: "Flower", amount: -19.99 },
  // { id: 2, text: "Salary", amount: 299.97 },
  // { id: 3, text: "Book", amount: -10 },
  // { id: 4, text: "Camera", amount: -150 },
]);

//Total
const total = computed(() =>
  parseInt(
    transactions.value
      .reduce((acc, transaction) => acc + transaction.amount, 0)
      .toFixed(2)
  )
);

//Income
const income = computed(() =>
  parseInt(
    transactions.value
      .filter((transaction) => transaction.amount > 0)
      .reduce((acc, transaction) => acc + transaction.amount, 0)
      .toFixed(2)
  )
);
//Expenses
const expenses = computed(() =>
  parseInt(
    transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((acc, transaction) => acc + transaction.amount, 0)
      .toFixed(2)
  )
);

//Add transaction
const handleTransactionSubmitted = (payload) => {
  transactions.value.push({ id: generateUniqueId(), ...payload });
  saveTransactionsToLocalStorage();
  toast.success("Transaction added!");
};

//Save into Local Storage
const saveTransactionsToLocalStorage = () =>
  localStorage.setItem("transactions", JSON.stringify(transactions.value));

//Generate ID
const generateUniqueId = () => Math.floor(Math.random() * 1000000);

//Delete transaction
function handleTransactionDeleted(id) {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  toast.success("Transaction deleted");
}
</script>
<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList
      :transactions="transactions"
      @transaction-deleted="handleTransactionDeleted"
    />
    <TransactionForm @transaction-submitted="handleTransactionSubmitted" />
  </div>
</template>
