<script setup>
import PackageSelector from "@/components/SelectionScreen/PackageSelector.vue";
import OrderTypeSelector from "@/components/SelectionScreen/OrderTypeSelector.vue";
import packageTypeEnum from "@/constants/packageTypeEnum.js";
import {ref, watch} from "vue";

const selectedOrderType=ref(null);
const selectedPackage=ref(null);
const emit = defineEmits(['onOrderTypeChange', 'onPackageChange'])

watch(selectedOrderType, onOrderTypeChange)
watch(selectedPackage, onPackageChange)
function onOrderTypeChange(_selectedOrderType){
  selectedPackage.value=null
  emit("onOrderTypeChange", _selectedOrderType)
}
function onPackageChange(_selectedPackage){
  emit("onPackageChange", _selectedPackage)
}
</script>

<template>
  <div class="listCart">
    <OrderTypeSelector v-model="selectedOrderType"/>
    <div v-if="selectedOrderType">
      <PackageSelector v-model="selectedPackage" :selected-order-type="selectedOrderType"/>
    </div>
    <p v-if="selectedOrderType === packageTypeEnum.membresia" class="parra">
      Tu <span>membresía</span> se renovará mensualmente de forma
      atomática.</p>
  </div>
</template>

<style scoped>

</style>