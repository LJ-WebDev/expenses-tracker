<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="supplier">Supplier</label>
      <input
        type="text"
        id="supplier"
        v-model="supplier"
        placeholder="Enter supplier name ... "
      />
    </div>
    <div class="form-control">
      <label for="text">Product/Service</label>
      <input
        type="text"
        id="product-service"
        v-model="productService"
        placeholder="Enter product/service ... "
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        v-model="amount"
        placeholder="Enter amount ... "
      />
    </div>
    <div class="form-control">
      <label for="date">Date <br /> </label>
      <input
        type="text"
        id="date"
        v-model="date"
        placeholder="Enter date ... "
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const supplier = ref('');
const productService = ref('');
const amount = ref('');
const date = ref(new Date().toISOString().slice(0, 10));

const emit = defineEmits('transactionSubmitted');

const toast = useToast();

const onSubmit = () => {
  if (!productService.value || !amount.value) {
    toast.error('Both fields must be filled');
    return;
  }

  const transactionData = {
    supplier: supplier.value,
    productService: productService.value,
    amount: parseFloat(amount.value),
    date: date.value,
  };

  emit('transactionSubmitted', transactionData);

  supplier.value = '';
  productService.value = '';
  amount.value = '';

  console.log(localStorage.getItem('transactions'));
};
</script>
