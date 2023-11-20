<template>
  <Header />

  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @transaction-deleted="deleteTransaction"
    />
    <AddTransaction @transaction-submitted="addTransaction" />
  </div>
</template>

<script setup>
import Header from "@/components/Header.vue";
import Balance from "@/components/Balance.vue";
import IncomeExpenses from "@/components/IncomeExpenses.vue";
import TransactionList from "@/components/TransactionList.vue";
import AddTransaction from "@/components/AddTransaction.vue";

import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const data = JSON.parse(localStorage.getItem("transactions"));

  if (data) {
    transactions.value = data;
  }
});

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0)
  .toFixed(2);
});

const income = computed(() => {
  return transactions.value
    .filter((t) => t.amount > 0)
    .reduce((acc, t) => {
      return acc + t.amount;
    }, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((t) => t.amount < 0)
    .reduce((acc, t) => {
      return acc + t.amount;
    }, 0)
    .toFixed(2);
});


const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

const addTransaction = (data) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: data.text,
    amount: data.amount,
  });

  saveToLocalStorage();

  toast.success("Transaction added");
};

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((t) => t.id !== id);

  saveToLocalStorage();

  toast.success("Transaction deleted");
};

const saveToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};
</script>
