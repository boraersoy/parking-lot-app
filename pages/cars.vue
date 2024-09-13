<script setup lang="ts">


import { ref, onMounted } from 'vue';
  
  const url = 'http://localhost:3001'
  const cars = ref([]);
  const columns = [
    { key: 'licensePlate', label: 'LicensePlate' },
    { key: 'arrivalDate', label: 'ArrivalDate' }
  ];


  onMounted(async () => {
    const response = await fetch(`${url}/cars`);
    if (response.ok) {
      cars.value = await response.json();
    } else {
      console.error('Failed to fetch cars');
    }
  });

</script>

<template>
    <div class="container mx-auto mt-5">
        
        <h2 class="text-2xl font-bold mb-4">Cars in the Parking Lot</h2>
        <UTable :columns="columns" :rows="cars">
        </UTable>
        <div class="mt-4">
            <NuxtLink to="/enter">

            <UButton color="violet" variant="solid" >Enter Car</UButton>
            </NuxtLink>
            <NuxtLink to="/deportcar" >

            <UButton color="orange" variant="solid" class="ml-4" >Depart Car</UButton>
            </NuxtLink>                    

        <NuxtLink to="viewdepartedcars">
            <UButton color="lime" variant="solid" class="ml-4" >View Departed Cars</UButton>
        </NuxtLink>
      </div>

    </div>




  </template>





