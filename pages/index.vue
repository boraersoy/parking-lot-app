<template>
    <div class="mt-4 ml-4">
       <NuxtLink to="Cars" >
           <UButton color="lime" variant="solid" >View Cars</UButton>
       </NuxtLink>    
           <NuxtLink to="/enter" class="ml-4">

           <UButton color="violet" variant="solid" >Enter</UButton>
           </NuxtLink>
           <NuxtLink to="/deportcar" class="ml-4">

           <UButton color="orange" variant="solid"  >Depart</UButton>
           </NuxtLink>                    

       <NuxtLink to="viewdeportedcars" class="ml-4">
           <UButton color="lime" variant="solid" >View Departed Cars</UButton>
       </NuxtLink>
     </div>
</template>