<script setup>
import packageTypeEnum from "@/constants/packageTypeEnum.js";
import routingStateEnum from "@/constants/routingStateEnum.js";
import {ref, watch} from "vue";

const props = defineProps(['selectedOrderType', 'selectedPackage', 'routerStatus'])
const showWarning=ref(false)
function calculateTotal(){


  if(props.selectedOrderType === packageTypeEnum.paquete){
    return props.selectedPackage?.pricePackage || 0
  }else if(props.selectedOrderType === packageTypeEnum.membresia){
    return props.selectedPackage?.priceMembership || 0
  }else{
    return "0"
  }
}
watch(props, updateShowWarning)

function updateShowWarning(){
  showWarning.value= props.routerStatus === routingStateEnum.selectingOrder && props.selectedOrderType === packageTypeEnum.membresia
}

</script>

<template>
  <div class="checkOut">
    <div class="subtotal">
      <p>SUBTOTAL:</p>
      <p>$<span id="subtotal">{{ calculateTotal() }}.00</span></p>
    </div>
    <div :style="!showWarning?'visibility:hidden':''" class="terminos">
      <p>Al proceder, aceptas los Términos y Condiciones y <br> reconoces que tu membresía
        <span>se renovará <br>automáticamente con cargo mensual.</span>
      </p>
    </div>
    <button v-if=" props.routerStatus === routingStateEnum.selectingOrder"
            :disabled="!props.selectedOrderType || !props.selectedPackage"
            @click="$emit('moveForward')">Confirmar pedido</button>
    <button v-if="props.routerStatus === routingStateEnum.clientDataForm"
            @click="$emit('moveForward')">Confirmar datos</button>

  </div>
</template>

<style scoped>

</style>