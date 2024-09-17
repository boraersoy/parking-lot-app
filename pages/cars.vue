<script setup lang="ts">


import { ref, onMounted } from 'vue';

const url = 'http://localhost:3001'
//const cars = ref([]);
const q = ref('')
const { pending, data: cars } = await useLazyAsyncData('cars', () => $fetch(`${url}/cars`))
const pageCount = 10
const page = ref(1)
const columns = [
  { key: 'licensePlate', label: 'LicensePlate', sortable: true },
  { key: 'arrivalDate', label: 'ArrivalDate', sortable: true }
];
const filteredRows = computed(() => {
  if (!q.value) {
    return cars.value
  }

  return cars.value.filter((car) => {
    return Object.values(car).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})
const paginatedRows = computed(() => {
  return filteredRows.value.slice((page.value - 1) * pageCount, (page.value) * pageCount)
})

function departCar(licensePlate) {
  console.log(`Departing car with license plate: ${licensePlate}`);
}
</script>

<template>
  <div class="container mx-auto mt-5">

    <h2 class="text-2xl font-bold mb-4">Cars in the Parking Lot</h2>
    <div class="flex px-3 py-3.5 border-b border-gray-200 dark:border-gray-700">
      <UInput v-model="q" placeholder="Filter license plate" />
    </div>
    <div class="">
      <UTable :columns="columns" :rows="paginatedRows">
      <template #actions-data="{ row }">
        <UButton color="orange" size="xs" class="ml-2" variant="solid" @click="departCar(row.licensePlate)">Depart
        </UButton>
      </template>
    </UTable>
    </div>

    <div class="flex justify-end px-3 py-3.5 border-t border-gray-200 dark:border-gray-700">
      <UPagination v-model="page" :page-count="pageCount" :total="cars.length" />
    </div>
    <div class="mt-4">
      <NuxtLink to="/enter">

        <UButton color="violet" variant="solid">Enter Car</UButton>
      </NuxtLink>
      <NuxtLink to="/deportcar">

        <UButton color="orange" variant="solid" class="ml-4">Depart Car</UButton>
      </NuxtLink>

      <NuxtLink to="viewdepartedcars">
        <UButton color="lime" variant="solid" class="ml-4">View Departed Cars</UButton>
      </NuxtLink>
    </div>

  </div>




</template>
