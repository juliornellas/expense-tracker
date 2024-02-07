<script setup>
defineProps({
  total: {
    type: Number,
    required: true,
  },
});
</script>
<template>
  <h2>Balance</h2>
  <h4>${{ total }}</h4>
</template>
