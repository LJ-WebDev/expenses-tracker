<template>
  <h3>History</h3>
  <div class="ul-wrapper">
    <ul id="list" class="list">
      <!-- <li class="minus">
      Cash <span>-$400</span><button class="delete-btn">x</button>
    </li>
    <li class="plus">
      Paycheck <span>+$400</span><button class="delete-btn">x</button>
    </li> -->

      <li
        v-for="transaction in transactions"
        :key="transaction.id"
        :class="transaction.amount < 0 ? 'minus' : 'plus'"
        @click="deleteTransaction(transaction.id)"
      >
        {{ transaction.productService }}
        <span>{{ formatCurrency(transaction.amount) }}</span>
        <button class="delete-btn">X</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

const emit = defineEmits(['transactionDeleted']);

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
  formatCurrency: {
    type: Function,
    required: true,
  },
});

const deleteTransaction = (id) => {
  emit('transactionDeleted', id);
};
</script>
