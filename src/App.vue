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
    function setRouterStatus(route){
      routerStatus.value=route
    }

    // Weird CSS fix
    const allwaysSelect = ref("option1");
    const allwaysSelect2 = ref("option2");

    return {
      isMainDrawerActive,
      selectedOrderType,
      selectedPackage,
      allwaysSelect,
      allwaysSelect2,
      routerStatus,
      resetCounter,
      toggleMainDrawer,
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
      <MembershipWarningScreen v-if="routerStatus===routingStateEnum.membershipWarning"/>
      <CheckoutCompleteScreen v-if="routerStatus === routingStateEnum.paymentSuccess" :selected-order-type="selectedOrderType" :selected-package="selectedPackage"/>
      <!--End of the routing-->

      {{selectedOrderType}}
      {{selectedPackage}}
      <SubtotalAndConfirm
          v-show="routerStatus!==routingStateEnum.membershipWarning && routerStatus!==routingStateEnum.paymentSuccess"
          :selected-order-type="selectedOrderType"
          :selected-package="selectedPackage"
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
