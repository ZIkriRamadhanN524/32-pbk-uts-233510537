<template>
  <div class="app">
    <h1>Rental PS - Daftar Penyewaan</h1>

    <div class="input-section">
      <input v-model="newRental" placeholder="Tambah nama penyewa..." @keyup.enter="addRental" />
      <button @click="addRental">Tambah</button>
    </div>

    <div class="filter-section">
      <button @click="filter = 'all'">Semua</button>
      <button @click="filter = 'active'">Belum Selesai</button>
      <button @click="filter = 'completed'">Selesai</button>
    </div>

    <ul class="rental-list">
      <li v-for="(rental, index) in filteredRentals" :key="index">
        <input type="checkbox" v-model="rental.completed" />
        <span :class="{ done: rental.completed }">{{ rental.name }}</span>
        <button @click="removeRental(index)">Hapus</button>
      </li>
    </ul>
  </div>
</template>
<!---->
<script setup>
import { reactive, ref, computed } from 'vue';

const rentals = reactive([]);
const newRental = ref('');
const filter = ref('all');

function addRental() {
  if (newRental.value.trim() !== '') {
    rentals.push({ name: newRental.value, completed: false });
    newRental.value = '';
  }
}

function removeRental(index) {
  rentals.splice(index, 1);
}

const filteredRentals = computed(() => {
  if (filter.value === 'active') {
    return rentals.filter(rental => !rental.completed);
  } else if (filter.value === 'completed') {
    return rentals.filter(rental => rental.completed);
  } else {
    return rentals;
  }
});
</script>
<!---->
<style scoped>
.app {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}
.input-section {
  display: flex;
  margin-bottom: 10px;
}
.input-section input {
  flex: 1;
  padding: 8px;
}
.input-section button {
  padding: 8px 12px;
  margin-left: 5px;
}
.filter-section {
  margin-bottom: 10px;
}
.filter-section button {
  margin-right: 5px;
}
.rental-list {
  list-style: none;
  padding: 0;
}
.rental-list li {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}
.rental-list span {
  flex: 1;
  margin-left: 8px;
}
.rental-list span.done {
  text-decoration: line-through;
  color: gray;
}
.rental-list button {
  margin-left: 8px;
}
</style>
