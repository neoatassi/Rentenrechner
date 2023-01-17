<script setup>
import { ref, computed } from 'vue'
  
  
  // Fixed factors
  const rentenAlter = 63
  const Zinsenszins = 2.7
  // Grundbedarf           =  80 %  vom Nettoeinkommen nach dem Zinsenszins
  // Staatliche Rente      =  48 %  vom Nettoeinkommen nach dem Zinsenszins
  
  const bday = ref(null)
  const netIncome = ref(null)
  
  const bdayCheck = ref(false)
  const nettocheck = ref(false)
  
  const compoundedIncome = computed(() => {
    return (netIncome.value * Math.pow(1 + Zinsenszins / 100, rentenAlter - bday.value)) - netIncome.value
  })
  const basicNeed = computed(() => {
    return compoundedIncome.value * 0.8
  })
  const statePension = computed(() => {
    return compoundedIncome.value * 0.48
  })
  
  const gap = computed(() => {
    return basicNeed.value - statePension.value
  })
  
  
  function setF(e) {
    bday = e
    bdayCheck = true
  }
  function setC($netIncome, $bday) {
    //netIncome.value = v
    compoundedIncome.value = (netIncome * Math.pow(1 + 2.7 / 100, 63 - bday)) - netIncome
    //c.value.determined = true
    nettocheck = true
  }
  
</script>
<template>
<!--form @submit.prevent="onSubmit"-->
<div class="bg-white">
  <div>
    <h3>Dein Alter</h3>
    <!--input type="number" :value="bday" @change="bdayCheck = true"-->
    <input v-model="bday" @change="bdayCheck = true" placeholder="">
  </div>
  
  <div v-if="bdayCheck">
    
      <h3>Dein Nettoeinkommen</h3>
      <input v-model="netIncome" @change="nettocheck = true" placeholder="">
      <!--input type="number" :value="netIncome" @change="nettocheck = true"-->
      <!--button @click="setC">Abschicken</button-->  
  </div>
<!--/form-->
  
  <div v-if="nettocheck">
    <h3>Nach dem Zinsenszins</h3>
    <h4> {{ compoundedIncome.toFixed(2) }} €</h4>

    <h3>Grundbedarf</h3>
    <h4>{{ basicNeed.toFixed(2) }} €</h4>

    <h3>Staatliche Rente</h3>
    <h4>{{ statePension.toFixed(2) }} €</h4>

    <h3>Lücke</h3>
    <h4>{{ gap.toFixed(2) }} €</h4>
  </div>
</div>
  
</template>