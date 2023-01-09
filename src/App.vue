
<script setup>
//Hay 2 maneras de usar el state en vue, con ref y reactive,
//ref toma valores primitivov
//reactive toma objetos

//sintaxis larga para eventos
// v-on:input="handleChange"
import { ref, computed, watch, camelize } from 'vue'
import Header from './components/Header.vue';
import Button from './components/Button.vue'
import {calcularTotalPagar} from './helpers'

//Estados
const cantidad = ref(10000)
const meses = ref(6)
const total = ref(0)


const MIN = 0
const MAX = 20000
const STEP = 100

// const formatearDinero = computed(() => {
//   const formatter = new Intl.NumberFormat('en-US', {
//     style: 'currency',
//     currency: 'USD'
//   });
//   return formatter.format(cantidad.value)
// });

const formatearDinero = (valor) => {
  const formatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  });
  return formatter.format(valor)
}

watch([cantidad, meses], ()=>{
  total.value = calcularTotalPagar(cantidad.value, meses.value)
})

const pagoMensual = computed(()=>{
  return total.value / meses.value
})


const handleChangeDecremento = ()=>{
  const valor = cantidad.value - STEP
  if(valor < MIN){
    alert('Cantidad no valida')
    return;

  } 

  cantidad.value = valor
  
}

const handleChangeIncremento = ()=>{
  const valor = cantidad.value + STEP
  if(valor > MAX){
    alert('Cantidad no valida')
    return;

  } 

  cantidad.value = valor
  
}


// const handleChange = (e) => {
//   cantidad.value = Number(e.target.value)
// }




  // console.log(cantidad.value)
  // console.log(state.cantidad)

</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow-md p-10 rounded-lg">
    <Header />
    <div class="flex justify-between mt-10">
     <Button 
     :operador="'-'"
     @fn="handleChangeDecremento"
     />
     <Button
     :operador="'+'"
     @fn="handleChangeIncremento"
     />

    </div>
    <div class="my-5">
      <!-- <input type="range" :min="MIN" :max="MAX" :step="STEP" :value="cantidad" class="w-full bg-gray-300 accent-lime-500 hover:accent-lime-600" @input="handleChange"> -->
      <input type="range" :min="MIN" class="w-full bg-gray-300 accent-lime-500 hover:accent-lime-600" :max="MAX"
        :step="STEP" v-model.number="cantidad">
      <p class="text-center my-10 text-5xl font-extrabold text-lime-600">{{ formatearDinero(cantidad) }}</p>
      <!-- <p v-text="`$${cantidad}`"></p> -->
      <h2 class="text-2xl font-extrabold texr-gray-500 text-center">Eligue un <span class="text-lime-400">plazo</span> a pagar</h2>
      <select :value="meses" v-model.number="meses" class="w-full p-2 bg-white border border-gray-300 rounded-lg text-center text-xl font-bold text-gray-500 mt-5">
        <option value="6">6 meses</option>
        <option value="12">12 meses</option>
        <option value="24">24 meses</option>
      </select>
    </div>
    <div v-if="total > 0" class="my-5 space-y-3 bg-gray-50 p-5 ">
      <h2 class="text-2xl font-extrabold text-gray-500 text-center">
        resumen <span class="text-lime-400">de pagos</span>
      </h2>
      <p class="text-xl text-gray-500 text-center font-bold">Meses: <span class="text-lime-500">{{ meses }}</span></p>
      <p class="text-xl text-gray-500 text-center font-bold">Total a pagar: <span class="text-lime-500">{{ formatearDinero(total) }}</span></p>
      <p class="text-xl text-gray-500 text-center font-bold">Mensuales: <span class="text-lime-500">{{ formatearDinero(pagoMensual) }}</span></p>
    </div>
    <p v-else class="text-center font-semibold text-gray-500">Añade una cantidad y un plazo a pagar</p>
  </div>
</template>
