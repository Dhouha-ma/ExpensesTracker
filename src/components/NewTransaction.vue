<script setup lang="ts">
import type { Transaction } from "@/models/expenseTracker";
import { ref } from "vue";

const emit = defineEmits<{
  "new-transaction": [newTransaction: Transaction];
}>();

const text = ref<string>("");
const amount = ref<number | null>(null);

const generateId = () => {
  return Math.floor(Math.random() * 1000000);
};

const onSubmit = () => {
  if (!text.value.trim() || amount.value === null) {
    return;
  }

  const newTransaction: Transaction = {
    id: generateId(),
    text: text.value,
    amount: amount.value,
  };

  emit("new-transaction", newTransaction);

  text.value = "";
  amount.value = null;
};
</script>

<template>
  <h3>Add new transaction</h3>

  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>

    <div class="form-control">
      <label for="amount">Amount </label>
      <input
        type="number"
        id="amount"
        placeholder="Enter amount..."
        v-model.number="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<style scoped></style>
