<script setup lang="ts">
import type { Transaction } from "@/models/expenseTracker";

defineProps<{
  transactions: Transaction[];
}>();

const emit = defineEmits<{
  "transaction-deleted": [transactionDeleted: number];
}>();

const deleteTransaction = (transactionId: number) => {
  emit("transaction-deleted", transactionId);
};
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="(transaction.amount ?? 0) < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }}
      <span>${{ Math.abs(transaction.amount ?? 0).toFixed(2) }}</span>

      <button class="delete-btn" @click="deleteTransaction(transaction.id)">
        x
      </button>
    </li>
  </ul>
</template>

<style scoped></style>
