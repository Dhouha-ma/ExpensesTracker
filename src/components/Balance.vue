<script setup lang="ts">
defineProps<{
  balance: number;
}>();
</script>

<template>
  <h3>Your Balance:</h3>
  <p id="balance">${{ balance.toFixed(2) }}</p>
</template>

<style></style>
