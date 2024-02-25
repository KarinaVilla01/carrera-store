<script setup>
  import packageTypeEnum from "@/constants/packageTypeEnum.js";
  import {membreshipIcon, packageIcon} from "@/constants/imageLocations.js";

  const props = defineProps(['selectedOrderType', 'selectedPackage'])

  function optionImageSrc(){
    if(props.selectedOrderType === packageTypeEnum.membresia) {
      return membreshipIcon
    }
    return packageIcon
  }

  function calculateTotal(){
    if(props.selectedOrderType === packageTypeEnum.paquete){
      return props.selectedPackage?.pricePackage
    }else if(props.selectedOrderType === packageTypeEnum.membresia){
      return props.selectedPackage?.priceMembership
    }else{
      return "0"
    }
  }
</script>

<template>
  <div class="paymentPage">
  <div>
    <img src="../../_assets/logoCarrera.png" alt="">
  </div>
  <div>
    <img src="../../_assets/Confirmation Ico.png" alt="">
  </div>
  <h1>${{ calculateTotal() }}.00</h1>
  <h4> ¡Pago Exitoso!</h4>
  <div class="card-payment">
    <div>
      <div class="intoCardPayment">
        <h4>CONCEPTO: </h4>
        <div>
          <p v-if="props.selectedOrderType === 'Membresía'"> Membresía Mensual.</p>
          <p v-if="props.selectedOrderType === 'Paquete'"> Paquete.</p>
        </div>
      </div>
    </div>
    <div class="seccion1">
      <div>
        <p>Pagaste a:</p>
        <h4>CARRERA CARWASH</h4>
      </div>
      <img class="flagsImg" src="../../_assets/flags.png" alt="">
    </div>
    <div class="seccion1">
      <div>
        <p>Servicio:</p>
        <h4>{{ props.selectedPackage?.name }}</h4>
      </div>
      <img class="flagsImg" :src="optionImageSrc('Paquete')" alt="">
    </div>
    <div class="seccion1">
      <div>
        <p>Método de pago:</p>
      </div>
      <img class="flagsImg" src="../../_assets/visa-logo.png" alt="">
    </div>
    <div class="total">
      <h4>TOTAL: <span>${{ calculateTotal() }}.00</span></h4>
    </div>
  </div>
  <button @click="$emit('close')">TERMINAR</button>
  </div>
</template>

<style scoped>

</style>