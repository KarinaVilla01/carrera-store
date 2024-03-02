<script setup>
import {ref, watch} from "vue";

const clientData=defineModel({default:{}})
const paymentType=ref( 0)
const location=ref(35090)
const firstName=ref("")
const lastName=ref("")
const email=ref("")
const phone=ref("")

watch(paymentType, ()=>{
  clientData.value={...clientData.value, paymentType}
})
watch(location, ()=>{
  clientData.value={...clientData.value, location}
})
watch(firstName, ()=>{
  clientData.value={...clientData.value, firsNme: firstName}
})
watch(lastName, ()=>{
  clientData.value={...clientData.value, lastName}
})
watch(email, ()=>{
  clientData.value={...clientData.value, email}
})
watch(phone, ()=>{
  clientData.value={...clientData.value, phone}
})
</script>

<template>
<div>
  <select name="paymentType" v-model="paymentType">
    <option value=0>Visa/MasterCard</option>
    <option value=2>Amex</option>
  </select><br/>
  <select name="location" v-model="location">
    <option value=35090>Hermosillo/Quiroga</option>
    <option value=35145>Mexicali</option>
  </select><br/>
  <label>Nombre</label>
  <input v-model="firstName"/><br/>
  <label>Apellido</label>
  <input v-model="lastName"/><br/>
  <label>Correo</label>
  <input v-model="email"/><br/>
  <label>Telefono</label>
  <input v-model="phone"/>
</div>
</template>

<style scoped>

</style>