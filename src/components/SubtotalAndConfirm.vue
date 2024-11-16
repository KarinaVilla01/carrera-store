<script setup>
import packageTypeEnum from "@/constants/packageTypeEnum.js";
import routingStateEnum from "@/constants/routingStateEnum.js";
import {nextTick, ref, watch} from "vue";

const props = defineProps(['selectedOrderType', 'selectedPackage', 'routerStatus', 'clientData', 'randomRef'])
const emit = defineEmits(['moveForward'])
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

async function submitItem(evt){
  evt.preventDefault();
  const body = JSON.stringify({
    packageId: props.selectedPackage._id,
    orderType: props.selectedOrderType,
    cardType: props.clientData.paymentType,
    location: props.clientData.location,
    firstName: props.clientData.firstName,
    lastName: props.clientData.lastName,
    email: props.clientData.email,
    phone: props.clientData.phone,
    ref: props.randomRef
  })
  const createdPurchaseOrder=await (await fetch(
      "https://api.carreracarwash.com/Prosepago/purchase-order",
      {mode: 'cors',  headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },method:'POST', body})).json()
  await nextTick(()=>{
    document.getElementById("submitForm").submit();
    emit('moveForward')
  })

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
        id="submitForm"
        method="post"
        target="_blank"
        v-if="props.routerStatus === routingStateEnum.clientDataForm"
    >
      <input name="t" id="t" type="hidden" :value="props.clientData.location"/>
      <input name="medio_pago" id="medio_pago" type="hidden" :value="props.clientData.paymentType"/>
      <input name="msi_val" id="msi_val" type="hidden" value="0"/>
      <input name="nom" id="nom" type="hidden" :value="`${props.clientData.firstName} ${props.clientData.lastName}`"/>
      <input name="con" id="con" type="hidden" :value="`${props.selectedOrderType === packageTypeEnum.paquete?'Paquete':'Membresia'} ${props.selectedPackage.name}`"/>
      <input name="ref" id="ref" type="hidden" :value="props.randomRef"/>
      <input name="imp" id="imp" type="hidden" :value="props.selectedOrderType === packageTypeEnum.paquete? props.selectedPackage.pricePackage:props.selectedPackage.priceMembership"/>
      <input name="ema" id="ema" type="hidden" :value="props.clientData.email"/>
      <input name="urlok" id="urlOk" type="hidden" value="https://carreracarwash.com"/>
      <input name="urlko" id="urlKo" type="hidden" value="https://carreracarwash.com"/>
      <input name="autoBack" id="autoBack" type="hidden" value="1"/>
    </form>
    <button v-if="props.routerStatus === routingStateEnum.clientDataForm"
            :disabled="props.clientData.errors"
            @click="submitItem">Confirmar pedido</button>

  </div>
</template>

<style scoped>
@import "../assets/main.css";
</style>
