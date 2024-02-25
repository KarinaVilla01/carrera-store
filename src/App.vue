<script>
import {ref, toRefs} from "vue"
import OrderTypeSelector from "@/components/SelectionScreen/OrderTypeSelector.vue";
import CheckoutCompleteScreen from "@/components/CheckoutCompleteScreen/CheckoutCompleteScreen.vue";
import PackageSelector from "@/components/SelectionScreen/PackageSelector.vue";
import SubtotalAndConfirm from "@/components/SubtotalAndConfirm.vue";
import routingStateEnum from "@/constants/routingStateEnum.js";
import ShoppingCartHeader from "@/components/ShoppingCartHeader.vue";
import SelectionScreen from "@/components/SelectionScreen/SelectionScreen.vue";
import MembershipWarningScreen from "@/components/MembershipWarningScreen/MembershipWarningScreen.vue";
import packageTypeEnum from "@/constants/packageTypeEnum.js";

export default {
  computed: {
    routingStateEnum() {
      return routingStateEnum
    },
  },
  components: {
    MembershipWarningScreen,
    SelectionScreen,
    ShoppingCartHeader, SubtotalAndConfirm, PackageSelector, CheckoutCompleteScreen, OrderTypeSelector},
  setup() {

    //User selections
    const selectedOrderType = ref(null);
    const selectedPackage = ref(null)

    const resetCounter=ref(0)

    // State of the app
    const isMainDrawerActive = ref(false);
    const routerStatus=ref(routingStateEnum.selectingOrder)
    function toggleMainDrawer(){
      if(isMainDrawerActive.value){
        selectedPackage.value=null
        selectedOrderType.value=null

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
            routerStatus.value=routingStateEnum.paymentSuccess
          }
          break;
        case routingStateEnum.membershipWarning:
          routerStatus.value=routingStateEnum.paymentSuccess
      }
    }

    return {
      isMainDrawerActive,
      selectedOrderType,
      selectedPackage,
      routerStatus,
      resetCounter,
      toggleMainDrawer,
      moveForward
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
          :router-status="routerStatus"
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
