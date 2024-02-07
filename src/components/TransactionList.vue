<script setup>
defineProps({
  transactions: {
    type: Array,
    required: false,
  },
});

const emit = defineEmits(["transaction-deleted"]);

function deleteTransaction(id) {
  emit("transaction-deleted", id);
}
</script>
<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span> ${{ transaction.amount }}</span>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">
        x
      </button>
    </li>
  </ul>
</template>
