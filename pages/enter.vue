<script setup>
import { ref, reactive } from 'vue'

const state = reactive({
  licensePlate: ''
})

const form = ref()
const validate = (state) => {
  const errors = []
  if (!state.licensePlate) errors.push({ path: 'licensePlate', message: 'Required' })
  return errors
}
async function onSubmit() {
  form.value.clear()
  try {
    const response = await $fetch('http://localhost:3001/cars', {
      method: 'POST',
      body: JSON.stringify({ licensePlate: state.licensePlate }),
      headers: {
        'Content-Type': 'application/json'
      }
    })
    alert('Car registered successfully');
    state.licensePlate = ''; // Clear the input after successful registration
    // Handle the response if needed
  } catch (error) {
  alert(error.data);
}
}
</script>


<template>
  <UForm ref="form" :state="state" :validate="validate" class="fixed top-0 left-0 p-4"@submit="onSubmit">
    <UFormGroup label="licensePlate" name="licensePlate">
      <UInput v-model="state.licensePlate" />
    </UFormGroup>


    <UButton type="submit" class="mt-4">
      Enter Car
    </UButton>

    <NuxtLink to="/cars">

    <UButton color="violet" variant="solid" class="ml-4">View Cars</UButton>
    </NuxtLink>
  </UForm>
</template>