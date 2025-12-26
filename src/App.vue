<template>
  <Header />
  <div class="container">
    <div>
      <Balance :total="+total" :formatCurrency="formatCurrency" />
      <IncomeExpenses
        :income="+income"
        :expenses="+expenses"
        :formatCurrency="formatCurrency"
      />

      <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
    <div>
      <TransactionList
        :transactions="transactions"
        :formatCurrency="formatCurrency"
        @transactionDeleted="handleTransactionDeleted"
      />
    </div>
  </div>
  <Footer :transactions="transactions" />
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import Footer from './components/Footer.vue';

import { useToast } from 'vue-toastification';

import { ref, computed, onMounted } from 'vue';

const toast = useToast();

const transactions = ref([]);

const formatter = new Intl.NumberFormat('en-AU', {
  style: 'currency',
  currency: 'AUD',
});

const formatCurrency = (value) => {
  return value < 0
    ? `-${formatter.format(Math.abs(value))}`
    : formatter.format(value);
};

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// Add transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: Date.now(),
    supplier: transactionData.supplier,
    productService: transactionData.productService,
    amount: transactionData.amount,
    date: transactionData.date,
  });

  saveTransactionsToLocalStorage();

  toast.success('Transaction added');
};

// Generate unique ID
// const generateUniqueId = () => {
//   return Math.floor(Math.random() * 1000);
// };

// Delete transaction
const handleTransactionDeleted = (id) => {
  if (confirm('Are you sure you want to delete this item?')) {
    transactions.value = transactions.value.filter(
      (transaction) => transaction.id !== id
    );
  } else {
    return;
  }

  saveTransactionsToLocalStorage();

  toast.success('Transaction deleted');
};

// Save to localStorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};
</script>
