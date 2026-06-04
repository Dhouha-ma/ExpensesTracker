<script setup lang="ts">
defineProps<{
  income: number;
  expense: number;
}>();
</script>

<template>
  <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <p id="money-plus" class="money plus">+${{ income.toFixed(2) }}</p>
    </div>
    <div>
      <h4>Expense</h4>
      <p id="money-minus" class="money minus">-${{ expense.toFixed(2) }}</p>
    </div>
  </div>
</template>

<style scoped></style>
