<script setup>
import routingStateEnum from "@/constants/routingStateEnum.js";

const props = defineProps(["routeStatus"])

function messageTop(){
  switch (props.routeStatus){
    case routingStateEnum.membershipWarning:
      return "ATENCIÓN"
    case routingStateEnum.selectingOrder:
    case routingStateEnum.clientDataForm:
    case routingStateEnum.checkout:
      return "CARRITO DE COMPRA"
    default:
      return ""
  }
}

</script>

<template>
  <div class="cart-header">
    <i v-if="props.routeStatus!==routingStateEnum.membershipWarning && props.routeStatus!==routingStateEnum.paymentSuccess" class="fa-solid fa-bag-shopping"></i>
    <i v-if="props.routeStatus===routingStateEnum.membershipWarning" class="fa-solid fa-circle-exclamation"></i>
    <h1 >{{messageTop()}}</h1>
    <span id="closeBtn" class="close-btn" @click="$emit('close')">&times;</span>
  </div>
</template>

<style scoped>

</style>