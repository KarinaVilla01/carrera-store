<script setup>
import packagesData from "@/constants/packagesData.js";
import packageTypeEnum from "@/constants/packageTypeEnum.js";
const selectedPackage=defineModel()
const props = defineProps(["selectedOrderType"])

function selectPackage(_package){
  selectedPackage.value=_package
}
</script>

<template>
  <h4>SELECCIONA TU SERVICIO PREFERIDO</h4>
  <ul class="listCartInto">
    <li v-for="pack in packagesData" :key="pack._id" :style="{ backgroundImage: 'url(' + pack.image + ')' }"
        @click="selectPackage(pack)" :class=" selectedPackage?._id===pack._id ?'selectedOption' : ''">
      <a :href="'#/package/' + pack._id">
        <div class="packLetters">{{ pack.name }}</div>
        <div v-if="selectedOrderType === packageTypeEnum.paquete" class="packLetters">${{ pack.pricePackage }} <span>.00</span>
        </div>
        <div v-if="selectedOrderType === packageTypeEnum.membresia" class="packLetters">${{ pack.priceMembership }}<span>.00</span>
        </div>
      </a>
    </li>
  </ul>
</template>

<style scoped>

</style>