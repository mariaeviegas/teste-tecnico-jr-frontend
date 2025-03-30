<script setup>
import { ref } from 'vue';
import FormModal from './FormModal.vue';

const dialog = ref(false);
</script>

<template>
  <div class="searchAddBar">
    <input type="text" class="searchAddBar__input" placeholder="Pesquise por título..." />
    <button class="searchAddBar__button" @click="dialog = true">Adicionar</button>
  </div>

  <FormModal v-model="dialog" />
</template>

<style scoped>
.searchAddBar {
  width: 80%;
  height: fit-content;
  margin: 1.5rem auto 0rem;
  background-color: var(--primary-color);
  padding: 1rem 1rem;
  border-radius: 1rem;
  display: flex;
  justify-content: space-between;
}

.searchAddBar__input {
  width: 40%;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 1rem;
  font-size: 1rem;
  background-color: white;
}

.searchAddBar__button {
  width: fit-content;
  padding: 1rem 1.5rem;
  border: 2px solid white;
  border-radius: 0.75rem;
  background-color: transparent;
  font-size: 1rem;
  font-weight: bold;
  color: white;
  cursor: pointer;
  transition: 0.1s;
}

.searchAddBar__button:hover {
  background-color: var(--secondary-color);
  color: white;
  border: 2px solid var(--secondary-color);
}
</style>