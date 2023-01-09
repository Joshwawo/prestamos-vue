<script setup>

//  const props = defineProps(['operador']);
const props = defineProps({
  operador: String,
  
});



</script>

<template>

  <button type="button" @click="$emit('fn')" class="h-10 w-10 flex items-center justify-center font-bold bg-lime-500 rounded-full hover:outline-none
        hover:ring-2 hover:ring-offset-2 text-white text-2xl hover:ring-lime-500">
        {{ operador }}
  </button>

</template>