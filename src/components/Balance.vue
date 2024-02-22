<template>
  <h4>Saldo</h4>
  <h1 id="balance">Rp. {{ total }}</h1>
</template>

<script setup>
  import {defineProps} from 'vue';

  const props = defineProps({
    total: {
      type: Number,
      required: true,
    }
  });
</script>