<template>
  <footer>
    <div>
      <button class="btn import" @click="handleImport">Import JSON</button
      ><button class="btn export" @click="handleExport">Export JSON</button>
    </div>
    <h2>JSON file:</h2>
    <h3>Copy this into another document to back up this data</h3>
    <pre>{{ transactions }}</pre>
  </footer>
</template>

<script setup>
import { computed } from 'vue';
import { defineProps } from 'vue';

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(['import', 'export']);

const handleImport = () => {
  emit('import');
};

const handleExport = () => {
  emit('export');
};

// const prettyJson = computed(() => JSON.stringify(transactions.value, null, 2));
</script>

<style scoped>
footer div {
  display: flex;
  justify-content: space-around;
}

.btn {
  cursor: pointer;
  background-color: #ffcc00;
  box-shadow: inset -2px 0px 10px #ffcc00, 1px 1px 10px #ffcc00;
  color: #090040;
  max-width: 25%;
}

.btn:hover {
  background-color: #cf9c00;
}
</style>
