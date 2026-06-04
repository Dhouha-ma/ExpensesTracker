<script setup lang="ts">
import { computed, onMounted, ref } from "vue";

import Balance from "@/components/Balance.vue";
import IncomeExpense from "@/components/IncomeExpense.vue";
import TransactionHistory from "@/components/TransactionHistory.vue";
import type { Transaction } from "@/models/expenseTracker";
import NewTransaction from "@/components/NewTransaction.vue";

const transactions = ref<Transaction[]>([]);

transactions.value = [
  {
    id: 1,
    text: "Transaction 1",
    amount: 3,
  },
  {
    id: 2,
    text: "Transaction 2",
    amount: -5,
  },
];

const handleTransaction = (transaction: Transaction) => {
  transactions.value.push(transaction);

  saveTransactionToLocalStorage();
};

const saveTransactionToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};

const handleDeleteTransaction = (transactionId: number) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== transactionId,
  );

  saveTransactionToLocalStorage();
};

const balance = computed(() =>
  transactions.value.reduce(
    (total, transaction) => total + (transaction.amount ?? 0),
    0,
  ),
);

const income = computed(() =>
  transactions.value
    .filter((transaction) => (transaction.amount ?? 0) > 0)
    .reduce((total, transaction) => total + (transaction.amount ?? 0), 0),
);

const expense = computed(() =>
  transactions.value
    .filter((transaction) => (transaction.amount ?? 0) < 0)
    .reduce(
      (total, transaction) => total + Math.abs(transaction.amount ?? 0),
      0,
    ),
);

onMounted(() => {
  const savedTransactions = localStorage.getItem("transactions");

  if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions);
  }
});
</script>

<template>
  <div class="container">
    <h2>Expense Tracker</h2>
    <Balance :balance="balance" />
    <IncomeExpense :income="income" :expense="expense" />
    <TransactionHistory
      :transactions="transactions"
      @transaction-deleted="handleDeleteTransaction"
    />
    <NewTransaction @new-transaction="handleTransaction" />
  </div>
</template>

<style scoped></style>
