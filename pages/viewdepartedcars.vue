<script setup>



import { ref, onMounted, computed } from 'vue';
  
  const url = 'http://localhost:3001'
  const cars = ref([]);
  const columns = [
    { key: 'licensePlate', label: 'LicensePlate' },
    { key: 'arrivalDate', label: 'ArrivalDate' },
    {key: 'charge', label: 'Charge'}
  ];

  const totalRevenue = computed(() => {
    return cars.value.reduce((sum, car) => sum + (car.charge || 0), 0);
  })

  onMounted(async () => {
    const response = await fetch(`${url}/deported`);
    if (response.ok) {
      cars.value = await response.json();
    } else {
      console.error('Failed to fetch cars');
    }
  });

</script>

<template>
    <div class="container mx-auto mt-5">
        
        <h2 class="text-2xl font-bold mb-4">Departed Cars</h2>
        <UTable :columns="columns" :rows="cars">
        </UTable>
        <div class="mt-4 p-4 bg-blue-200 text-blue-800 rounded">
      <strong>Total Revenue:</strong> ${{ totalRevenue.toFixed(2) }}
    </div>
        <div class="flex justify-between mt-4">
            <NuxtLink to="/enter">

            <UButton color="violet" variant="solid" >Enter Car</UButton>
            </NuxtLink>
            <NuxtLink to="/deportcar">

            <UButton color="orange" variant="solid" >Depart Car</UButton>
            </NuxtLink>
            
        <NuxtLink to="cars">
            <UButton color="lime" variant="solid" >View Cars</UButton>
        </NuxtLink>                    
        </div>


    </div>




  </template>





