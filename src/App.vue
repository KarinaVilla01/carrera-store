<script>
import {ref} from "vue"
import OrderTypeSelector from "@/components/SelectionScreen/OrderTypeSelector.vue";
import CheckoutCompleteScreen from "@/components/CheckoutCompleteScreen/CheckoutCompleteScreen.vue";
import PackageSelector from "@/components/SelectionScreen/PackageSelector.vue";
import SubtotalAndConfirm from "@/components/SubtotalAndConfirm.vue";
import routingStateEnum from "@/constants/routingStateEnum.js";
import ShoppingCartHeader from "@/components/ShoppingCartHeader.vue";
import SelectionScreen from "@/components/SelectionScreen/SelectionScreen.vue";
import MembershipWarningScreen from "@/components/MembershipWarningScreen/MembershipWarningScreen.vue";
import packageTypeEnum from "@/constants/packageTypeEnum.js";
import MembershipFormScreen from "@/components/ClientDataFormScreen/ClientDataFormScreen.vue";
import CheckoutScreen from "@/components/CheckoutScreen/CheckoutScreen.vue";
import WaitingScreen from "@/components/WaitingScreen/WaitingScreen.vue";

export default {
  computed: {
    routingStateEnum() {
      return routingStateEnum
    },
  },
  components: {
    WaitingScreen,
    CheckoutScreen,
    MembershipFormScreen,
    MembershipWarningScreen,
    SelectionScreen,
    ShoppingCartHeader, SubtotalAndConfirm, PackageSelector, CheckoutCompleteScreen, OrderTypeSelector},
  setup() {

    function uuidv4() {
      return "10000000-1000-4000-8000-100000000000".replace(/[018]/g, c =>
          (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
      );
    }

    //User selections
    const selectedOrderType = ref(null);
    const selectedPackage = ref(null)
    const clientData=ref({})
    const randomRef=ref(uuidv4())

    const resetCounter=ref(0)



    // State of the app
    const isMainDrawerActive = ref(false);
    const routerStatus=ref(routingStateEnum.selectingOrder)
    function toggleMainDrawer(){
      if(isMainDrawerActive.value){
        selectedPackage.value=null
        selectedOrderType.value=null
        clientData.value={}
        randomRef.value=uuidv4()

        routerStatus.value=routingStateEnum.selectingOrder
        resetCounter.value++
      }
      isMainDrawerActive.value = !isMainDrawerActive.value;
    }

    function moveForward(){
      switch (routerStatus.value){
        case routingStateEnum.selectingOrder:
          if(selectedOrderType.value===packageTypeEnum.membresia){
            routerStatus.value=routingStateEnum.membershipWarning
          }else{
            routerStatus.value=routingStateEnum.clientDataForm
          }
          break;
        case routingStateEnum.membershipWarning:
          routerStatus.value=routingStateEnum.clientDataForm
          break;
        case routingStateEnum.clientDataForm:
          routerStatus.value=routingStateEnum.waitingOnPayment
          break;
        case routingStateEnum.waitingOnPayment:
          routerStatus.value=routingStateEnum.paymentSuccess
      }
    }

    return {
      isMainDrawerActive,
      selectedOrderType,
      selectedPackage,
      routerStatus,
      resetCounter,
      clientData,
      toggleMainDrawer,
      moveForward,
      randomRef
    };
  },

};
</script>

<template>
  <main>
    <div :class="{ 'cart-price': true, 'active': isMainDrawerActive }" :key="resetCounter">
      <ShoppingCartHeader :route-status="routerStatus" @close="toggleMainDrawer"/>
      <!--Initiate the routing-->
      <SelectionScreen
          v-if="routerStatus === routingStateEnum.selectingOrder"
          @on-order-type-change="(value)=>selectedOrderType=value"
          @on-package-change="(value)=>selectedPackage=value"
      />
      <MembershipWarningScreen
          v-if="routerStatus===routingStateEnum.membershipWarning"
          @move-forward="moveForward"
      />
      <MembershipFormScreen
          v-if="routerStatus===routingStateEnum.clientDataForm"
          v-model="clientData"
          @move-forward="moveForward"
      />
      <WaitingScreen
        v-if="routerStatus===routingStateEnum.waitingOnPayment"
      />
      <CheckoutScreen
          v-if="routerStatus===routingStateEnum.checkout"
          @move-forward="moveForward"
      />
      <CheckoutCompleteScreen
          v-if="routerStatus === routingStateEnum.paymentSuccess"
          :selected-order-type="selectedOrderType || null"
          :selected-package="selectedPackage || null"
          @close="toggleMainDrawer"
      />
      <!--End of the routing-->
      <SubtotalAndConfirm
          v-show="routerStatus!==routingStateEnum.paymentSuccess && routerStatus!==routingStateEnum.paymentFail"
          :selected-order-type="selectedOrderType || null"
          :selected-package="selectedPackage || null"
          :client-data="clientData"
          :router-status="routerStatus"
          :random-ref="randomRef"
          @move-forward="moveForward"
      />
    </div>
    <div id="button-create">
      <button id="buy" @click="toggleMainDrawer">Comprar</button>
    </div>
  </main>
</template>


<style scoped>
button:hover {
  transition: 0.3s;
  opacity: 0.8;
}
</style>
