<script setup>



import { ref, computed } from 'vue';

const url = 'http://localhost:3001'
const q = ref('')
const pageCount = 10
const page = ref(1)
const isFirstModalOpen = ref(false)
const isSecondModalOpen = ref(false)
const columns = [
  { key: 'licensePlate', label: 'LicensePlate', sortable: true },
  { key: 'arrivalDate', label: 'ArrivalDate', sortable: true },
  { key: 'charge', label: 'Charge', sortable: true }
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


const totalRevenue = computed(() => {
  return cars.value.reduce((sum, car) => sum + (car.charge || 0), 0);
})

const { pending, data: cars } = await useLazyAsyncData('cars', () => $fetch(`${url}/deported`))

</script>

<template>
  <div class="container mx-auto mt-5">

    <h2 class="text-2xl font-bold mb-4">Departed Cars</h2>
    <div class="flex px-3 py-3.5 border-b border-gray-200 dark:border-gray-700">
      <UInput v-model="q" placeholder="Filter license plate" />
    </div>
    <div>
      <UTable :columns="columns"
        :rows="paginatedRows" />
      <div class="flex justify-end px-3 py-3.5 border-t border-gray-200 dark:border-gray-700">
        <UPagination v-model="page" :page-count="pageCount" :total="cars.length" />
      </div>
    </div>
    <div class="mt-4 p-4 bg-blue-200 text-blue-800 rounded">
      <strong>Total Revenue:</strong> ${{ totalRevenue.toFixed(2) }}
    </div>
    <div class="flex justify-between mt-4">
      <UButton @click="isFirstModalOpen = true" color="violet" variant="solid">Enter Car</UButton>
        <UModal class="shadow-lg" v-model="isFirstModalOpen" :overlay="false">
          <Enter />
        </UModal>
        <UButton @click="isSecondModalOpen = true" color="orange" variant="solid" class="ml-4">Depart Car</UButton>
        <UModal v-model="isSecondModalOpen" :overlay="false">
        <Deportcar />
      </UModal>

      <NuxtLink to="cars">
        <UButton color="lime" variant="solid">View Cars</UButton>

      </NuxtLink>
    </div>


  </div>




</template>
