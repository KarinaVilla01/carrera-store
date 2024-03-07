<script setup>
import packageTypeEnum from "@/constants/packageTypeEnum.js";
import routingStateEnum from "@/constants/routingStateEnum.js";
import {ref, watch} from "vue";

const props = defineProps(['selectedOrderType', 'selectedPackage', 'routerStatus', 'clientData'])
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
    <form
        style="display: grid"
        action="https://ecom.prosepago.com/tvirtual.aspx"
        method="post"
        target="_blank"
        v-if="props.routerStatus === routingStateEnum.clientDataForm"
    >
      <input name="t" id="t" type="hidden" :value="props.clientData.location"/>
      <input name="medio_pago" id="medio_pago" type="hidden" :value="props.clientData.paymentType"/>
      <input name="msi_val" id="msi_val" type="hidden" value="0"/>
      <input name="nom" id="nom" type="hidden" :value="`${props.clientData.firstName} ${props.clientData.lastName}`"/>
      <input name="con" id="con" type="hidden" :value="`${props.orderType === packageTypeEnum.paquete?'Paquete':'Membresia'} ${props.selectedPackage.name}`"/>
      <input name="ref" id="ref" type="hidden" value="69420"/>
      <input name="imp" id="imp" type="hidden" :value="props.orderType === packageTypeEnum.paquete? props.selectedPackage.pricePackage:props.selectedPackage.priceMembership"/>
      <input name="ema" id="ema" type="hidden" :value="props.clientData.email"/>
      <input name="urlok" id="urlOk" type="hidden" value="https://google.com"/>
      <input name="urlko" id="urlKo" type="hidden" value="https://youtube.com"/>
      <input name="autoBack" id="autoBack" type="hidden" value="1"/>
      <input type="submit" value="Continuar al pago" onclick="alert('hello')" id="submit"  :disabled="props.clientData.errors">
    </form>

  </div>
</template>

<style scoped>

</style>
