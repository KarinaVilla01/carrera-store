<script setup>

import packageTypeEnum from "@/constants/packageTypeEnum.js";
import {membreshipIcon, packageIcon} from "@/constants/imageLocations.js";

const orderType = defineModel()
const options = [
  { id: 1, imgSrc: packageIcon, value: packageTypeEnum.paquete, label: 'Paquete' },
  { id: 2, imgSrc: membreshipIcon, value: packageTypeEnum.membresia, label: 'Membresía' }
]

function setupOrderType(value){
  orderType.value=value
}
</script>

<template>
  <h5>Elije entre <span>"Paquete"</span>
    para servicio único <br> o <span>"Membresía"</span>
    para beneficios continuos. </h5>
  <div class="paqMem">
    <h4>PAQUETE</h4>
    <h4>MEMBRESÍA</h4>
  </div>
  <div class="selection">
    <div v-for="option in options" :key="option"
         :class="{ 'selection-btn': true, 'selected': orderType === option.value }"
         @click="setupOrderType(option.value)">
      <img class="img-pagMem" :src="option.imgSrc" alt="imagen">
      <input type="radio" name="radio-card" v-model="orderType" :value="option.value" />
    </div>
  </div>

</template>

<style scoped>

</style>