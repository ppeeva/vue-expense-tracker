<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="t in transactions"
      :key="t.id"
      :class="t.amount > 0 ? 'plus' : 'minus'"
    >
      {{ t.text }} <span>{{ t.amount }} лв</span
      ><button @click="deleteTransaction(t.id)" class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["transaction-deleted"]);

const deleteTransaction = (id) => {
  emit("transaction-deleted", id);
};
</script>
