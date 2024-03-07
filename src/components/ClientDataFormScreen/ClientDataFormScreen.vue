<script setup>
import { ref, watch } from "vue";

const clientData = defineModel({ default: {} })
const paymentType = ref(0)
const location = ref(35090)
const firstName = ref("")
const lastName = ref("")
const email = ref("")
const phone = ref("")
const errors=ref([])

function updateData(){
  clientData.value={paymentType, location, firstName, lastName, email, phone, errors: !err}
}
function validateEmail(email) {
  var re = /\S+@\S+\.\S+/;
  return re.test(email);
}
// Mensajes de error
const firstNameError = ref('');
const lastNameError = ref('');
const emailError = ref('');
const phoneError = ref('');

const validateEmail = (email) => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
};

watch(paymentType, updateData)
watch(location, updateData)
watch(firstName, (newValue) => {
  updateData()
  firstNameError.value = newValue ? '' : 'El nombre es requerido';
})
watch(lastName, (newValue) => {
  updateData()
  lastNameError.value = newValue ? '' : 'El apellido es requerido';
})
watch(email, (newValue) => {
  updateData()
  emailError.value = validateEmail(newValue) ? '' : 'El correo no es válido';
})
watch(phone, (newValue) => {
  updateData()
  const regex = /^\d{10}$/;
  phoneError.value = regex.test(newValue) ? '' : 'El teléfono debe tener 10 dígitos';
})
updateData()
</script>

<template>
  <div class="card-form__inner">
    <div class="card-input">
      <h2>Datos de pago:</h2>
      <label for="cardMonth" class="card-input__label">Tipo de tarjeta</label>
      <div class="icon-input-center">
        <i class="fa-regular fa-credit-card"></i>
        <select name="paymentType" v-model="paymentType" class="card-input__input -select">
          <option value=0>Visa/MasterCard</option>
          <option value=2>Amex</option>
        </select>
      </div>
      <label for="cardMonth" class="card-input__label">Sucursal de compra</label>
      <div class="icon-input-center">
        <i class="fa-solid fa-location-dot"></i>
        <select name="location" v-model="location" class="card-input__input -select">
          <option value=35090>Hermosillo/Quiroga</option>
          <option value=35145>Mexicali</option>
        </select>
      </div>
      <label class="card-input__label">Nombre</label>
      <div class="input-container">
        <i class="fa fa-user"></i>
        <input v-model="firstName" type="text" class="card-input__input" placeholder="Javier"
          @blur="validateFirstName">
        </div>
        <div v-if="firstNameError" class="error">{{ firstNameError }}</div>
      <label class="card-input__label">Apellido</label>
      <div class="input-container">
        <i class="fa-regular fa-user"></i>
        <input v-model="lastName" type="text" class="card-input__input" placeholder="López Martínez"
          @blur="validateLastName">
        </div>
        <div v-if="lastNameError" class="error">{{ lastNameError }}</div>
      <label class="card-input__label">Correo</label>
      <div class="input-container">
        <i class="fa-solid fa-envelope"></i>
        <input v-model="email" type="email" class="card-input__input" placeholder="email@carreracarwash.com"
          @blur="validateEmail">
        </div>
        <div v-if="emailError" class="error">{{ emailError }}</div>
      <label class="card-input__label">Telefono</label>
      <div class="input-container">
        <i class="fa-solid fa-phone"></i>
        <input v-model="phone" type="text" maxlength="10" class="card-input__input" placeholder="55 123456789"
          @blur="validatePhone">
        </div>
        <div v-if="phoneError" class="error">{{ phoneError }}</div>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: red;
  font-size: 0.7em;
}</style>
