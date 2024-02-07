<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();
const emit = defineEmits(["transactionSubmitted"]);

const text = ref("");
const amount = ref("");

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("Both fields must be filled");
    return;
  }

  if (amount.value.includes(",")) {
    toast.error("Should only use dot(.) for amount");
    return;
  }

  if (
    Number(amount.value) !==
    (parseFloat(amount.value) || parseInt(amount.value))
  ) {
    toast.error("Amount must be a number");
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  emit("transactionSubmitted", transactionData);

  text.value = "";
  amount.value = "";
};
</script>
<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        v-model.trim="text"
        type="text"
        id="text"
        placeholder="Enter text..."
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />(negative - expense, positive - income)</label
      >
      <input
        v-model.trim="amount"
        type="text"
        id="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
