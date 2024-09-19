<script setup lang="ts">
const search = ref('')
const sort = ref({ column: 'licensePlate', direction: 'asc' as const })
const page = ref(1)
const pageCount = ref(10)
const pageTotal = ref(50)   //This value should be dynamic coming from the API 
const pageFrom = computed(() => (page.value - 1) * pageCount.value + 1)
const pageTo = computed(() => Math.min(page.value * pageCount.value, pageTotal.value))
const { data, status, error, refresh } = await useFetch('http://localhost:3001/cars', {
    query: {
    q: search.value,
    '_page': page.value,
    '_limit': pageCount.value,
    '_sort': sort.value.column,
    '_order': sort.value.direction
  },
  onResponse({ request, response, options }) {
    // Process the response data
    const header = response.headers;
    console.log(header)
},
})

</script>

<template>
    {{ data }}
</template>