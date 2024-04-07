<script setup>
import { ref, onMounted } from 'vue';
import Card from './components/Card.vue'
import { computed } from 'vue';


const dataFromApi = ref(null);
const filterState = ref(2);
const isLoading = ref(true);

async function fetchData() {
  try {
    // Simular un retraso de 1 segundo antes de obtener los datos para ver si funciona el loading
    await new Promise(resolve => setTimeout(resolve, 1000));
    const response = await fetch('https://run.mocky.io/v3/726d45d4-2b3f-4035-ad38-ce379c592690');
    if (!response.ok) {
      throw new Error('Error al obtener los datos');
    }
    dataFromApi.value = await response.json();
    console.log(dataFromApi.value)
    isLoading.value = false; 
  } catch (error) {
    console.error('Error al obtener los datos:', error);
  }
}
onMounted(fetchData);

function filterCards(state) {
  filterState.value = state;
}

// Filtrar datos según el estado
const filteredData = computed(() => {
  if (!dataFromApi.value || !filterState.value) {
    return [];
  }
  return dataFromApi.value.filter(item => item.state.id === filterState.value);
});
</script>


<template>
  <div class="container">
    <h5 class="title p-3 fw-bold">Ejercicio</h5>
    <div class="buttons-container">
      <button @click="filterCards(1)" :class="{ 'btn-custom-active fw-semibold ': filterState === 1, 'btn-custom-inactive fw-semibold ': filterState !== 1 }" class="btn me-2"><i class="bi bi-arrow-clockwise me-1"></i>Borrador</button>
      <button @click="filterCards(2)" :class="{ 'btn-custom-active fw-semibold ': filterState === 2, 'btn-custom-inactive fw-semibold ': filterState !== 2 }" class="btn me-2"><i class="bi bi-check-circle me-1"></i>Confirmadas</button>
      <button @click="filterCards(3)" :class="{ 'btn-custom-active fw-semibold ': filterState === 3, 'btn-custom-inactive fw-semibold ': filterState !== 3 }" class="btn me-2"><i class="bi bi-check-all me-1"></i>Visitadas</button>
    </div>
    <div v-if="isLoading" class="text-center mt-3">
      <p>Cargando...</p>
    </div>
    <Card v-if="!isLoading" v-for="item in filteredData" :key="item.id" 
      :name="item.names[0].name" 
      :id="item.id" 
      :address="item.address" 
      :date="item.date" 
      :state="item.state.id" 
      :timbrado="item.timbrado" 
      :cep="item.cep"
    />
  </div>
</template>

<style>
.container{
  background-color: #E9EDF8;
}
.title{
  color:#1f3450
}
.btn-custom-active {
  background-color: #1a2c43;
  color: #ffffff; 
}
.btn-custom-inactive {
  background-color: #d3d3d3; 
  color: #000000; 
}
</style>


