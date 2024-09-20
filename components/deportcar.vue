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
    const response = await $fetch('http://localhost:3001/deport', {
      method: 'POST',
      body: JSON.stringify({ licensePlate: state.licensePlate }),
      headers: {
        'Content-Type': 'application/json'
      }
    })
    alert(`Car deported. Charge: $${response.charge}`);


} catch (error) {
alert(error.data);
}
}
</script>


<template>
  <div class="p-6 bg-gray-50 rounded-lg shadow-md" >
  <UForm ref="form" class="h-36" :state="state" :validate="validate" @submit="onSubmit">
    <UFormGroup class="mt-2" label="Enter License Plate" name="licensePlate">
      <UInput v-model="state.licensePlate" />
    </UFormGroup>

    <UButton type="submit" class="mt-4">
      Depart Car
    </UButton>

  </UForm>
  </div>
</template>