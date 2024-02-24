<script setup>
import packageTypeEnum from "@/constants/packageTypeEnum.js";

const props = defineProps(["selectedOrderType, selectedPackage"])

function calculateTotal(){
  console.log(props)
  if(props.selectedOrderType === packageTypeEnum.paquete){
    return props.selectedPackage?.pricePackage || 0
  }else if(props.selectedOrderType === packageTypeEnum.membresia){
    return props.selectedPackage?.priceMembership || 0
  }else{
    return "0"
  }
}
</script>

<template>
  <div class="checkOut">
    {{props}}
    <div class="subtotal">
      <p>SUBTOTAL:</p>
      <p>$<span id="subtotal">{{ calculateTotal() }}.00</span></p>
    </div>
<!--    <button v-if="calculateTotal() !== 0 && confirmShopping === false"-->
<!--            @click="confirmShoppingCart()">Confirmar pedido</button>-->
<!--    <button v-if="confirmShopping === true && confirmCartPayment === false && activatePopup === false"-->
<!--            @click="confirmPaymentMethod()">REALIZAR EL PAGO</button>-->
<!--    <button v-if="confirmPayment === true && activatePopup === false && selectedOrderType === packageTypeEnum.membresia"-->
<!--            @click="activatedPopup">CONFIRMAR PAGO</button>-->
<!--    <button v-if="confirmPayment === true && selectedOrderType === packageTypeEnum.paquete" @click="autorizePayment">CONFIRMAR-->
<!--      PAGO</button>-->
<!--    <div v-if="confirmPayment === true && selectedOrderType === packageTypeEnum.membresia" class="terminos">-->
<!--      <p>Al proceder, aceptas los Términos y Condiciones y <br> reconoces que tu membresía-->
<!--        <span>se renovará <br>automáticamente con cargo mensual.</span>-->
<!--      </p>-->
<!--    </div>-->
  </div>
</template>

<style scoped>

</style>