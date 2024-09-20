<script lang="ts" setup>
// Columns
const columns = [
  { key: 'licensePlate', label: 'LicensePlate', sortable: true },
  { key: 'arrivalDate', label: 'ArrivalDate', sortable: true },

];
const isFirstModalOpen = ref(false)
const isSecondModalOpen = ref(false)
const url = 'http://localhost:3001'
const selectedColumns = ref(columns)
const columnsTable = computed(() => columns.filter((column) => selectedColumns.value.includes(column)))

// Selected Rows
const rows = ref([]) 



// Actions
const actions = [
  [{
    key: 'completed',
    label: 'Completed',
    icon: 'i-heroicons-check'
  }], [{
    key: 'uncompleted',
    label: 'In Progress',
    icon: 'i-heroicons-arrow-path'
  }]
]



const search = ref('')




// Pagination
const sort = ref({ column: 'licensePlate', direction: 'asc' as const })
const page = ref(1)
const pageCount = ref(10)
const pageTotal = ref(200) // This value should be dynamic coming from the API
const pageFrom = computed(() => (page.value - 1) * pageCount.value + 1)
const pageTo = computed(() => Math.min(page.value * pageCount.value, pageTotal.value))

// Data
const { data: todos, status } = await useLazyAsyncData<{
  licensePlate: string
  arrivalDate: string

}[]>('todos', () => ($fetch as any)(`http://localhost:3001/cars`, {
  query: {
    q: search.value,
    '_page': page.value,
    '_limit': pageCount.value,
    '_sort': sort.value.column,
    '_order': sort.value.direction
  }
}), {
  default: () => [],
  watch: [page, search, pageCount, sort]
})

</script>

<template>
  <UCard
    class="w-full"
    :ui="{
      base: '',
      ring: '',
      divide: 'divide-y divide-gray-200 dark:divide-gray-700',
      header: { padding: 'px-4 py-5' },
      body: { padding: '', base: 'divide-y divide-gray-200 dark:divide-gray-700' },
      footer: { padding: 'p-4' }
    }"
  >
    <template #header>
      <h2 class="font-semibold text-xl text-gray-900 dark:text-white leading-tight">
        Todos
      </h2>
    </template>

    <!-- Filters -->
    <div class="flex items-center justify-between gap-3 px-4 py-3">
      <UInput v-model="search" icon="i-heroicons-magnifying-glass-20-solid" placeholder="Search..." />
    </div>

    <!-- Header and Action buttons -->
    <div class="flex justify-between items-center w-full px-4 py-3">
      <div class="flex items-center gap-1.5">
        <span class="text-sm leading-5">Rows per page:</span>

        <USelect
          v-model="pageCount"
          :options="[3, 5, 10, 20, 30, 40]"
          class="me-2 w-20"
          size="xs"
        />
      </div>

      <div class="flex gap-1.5 items-center">
        

        <USelectMenu v-model="selectedColumns" :options="columns" multiple>
          <UButton
            icon="i-heroicons-view-columns"
            color="gray"
            size="xs"
          >
            Columns
          </UButton>
        </USelectMenu>

        
      </div>
    </div>

    <!-- Table -->
    <UTable
      v-model="rows"
      v-model:sort="sort"
      :rows="todos"
      :columns="columnsTable"
      :loading="status === 'pending'"
      sort-asc-icon="i-heroicons-arrow-up"
      sort-desc-icon="i-heroicons-arrow-down"
      sort-mode="manual"
      class="w-full"
    >
      

     
    </UTable>

    <!-- Number of rows & Pagination -->
    <template #footer>
      <div class="flex flex-wrap justify-between items-center">
        <div>
          <span class="text-sm leading-5">
            Showing
            <span class="font-medium">{{ pageFrom }}</span>
            to
            <span class="font-medium">{{ pageTo }}</span>
            of
            <span class="font-medium">{{ pageTotal }}</span>
            results
          </span>
        </div>

        <UPagination
          v-model="page"
          :page-count="pageCount"
          :total="pageTotal"
          :ui="{
            wrapper: 'flex items-center gap-1',
            rounded: '!rounded-full min-w-[32px] justify-center',
            default: {
              activeButton: {
                variant: 'outline'
              }
            }
          }"
        />
      </div>
    </template>
  </UCard>

  <div class="mt-4 p-4">
        <UButton class="ml-2" @click="isFirstModalOpen = true" color="violet" variant="solid">Enter Car</UButton>
        <UModal class="shadow-lg" v-model="isFirstModalOpen" :overlay="false">
          <Enter />
        </UModal>
        <UButton  @click="isSecondModalOpen = true" color="orange" variant="solid" class="ml-6">Depart Car</UButton>
        <UModal v-model="isSecondModalOpen" :overlay="false">
        <Deportcar />
      </UModal>

      <NuxtLink to="viewdepartedcars">
        <UButton  color="lime" variant="solid" class="ml-6">View Departed Cars</UButton>
      </NuxtLink>
    </div>
</template>