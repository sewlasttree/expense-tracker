<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Deskripsi</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount">Jumlah uang <br />
        <i style="font-size: 12px;" >Note: tambahkan tanda <span style="color: red;">minus (-)</span> di depan angka apabila expense</i>
      </label>
      <input type="text" id="amount" v-model="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
  import {ref} from 'vue';
  import {useToast} from 'vue-toastification';

  const text = ref('');
  const amount = ref('');

  const emit = defineEmits(['transactionSubmitted']);

  const toast = useToast();

  const onSubmit = () => {
    if(!text.value || !amount.value) {
      toast.error('Tidak boleh dikosongkan');
      return;
    }

    const transactionData = {
      text: text.value,
      amount: parseFloat(amount.value)
    }

    emit('transactionSubmitted', transactionData);

    text.value = '';
    amount.value = '';
  };
</script>